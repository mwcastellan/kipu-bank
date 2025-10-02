# KipuBank 🏦: Banco Descentralizado en Ethereum
## 👤 Autor
Marcelo Walter Castellan

## 📜 Descripción del Proyecto

**KipuBank** es un contrato inteligente de **banco descentralizado** desarrollado en **Solidity** para la red Ethereum. Permite a los usuarios **depositar** y **retirar** ETH de manera segura, actuando como una bóveda personal.

El contrato impone límites estrictos tanto para las transacciones individuales de retiro como para la capacidad total de capital que puede albergar el banco (Cap). Utiliza **errores personalizados** (Custom Errors) para proporcionar mensajes de fallo claros y eficientes, y **eventos** para facilitar el seguimiento de las transacciones en la blockchain.

---

## ✨ Características Principales

* **Depósitos de ETH:** Los usuarios pueden depositar ETH en su bóveda personal dentro del contrato.
* **Retiros de ETH:** Permite a los usuarios retirar su balance de ETH.
* **Límite de Retiro por Transacción:** Existe un límite de **1 ETH** por retiro para mitigar riesgos.
* **Capital Máximo (Cap):** El banco tiene un capital máximo de **1000 ETH**. Cualquier depósito que intente superar este límite será revertido.
* **Seguridad y Eficiencia:** Implementación de un modificador (`nonZeroValue`) y uso de `call` para la transferencia de ETH, asegurando un mecanismo de retiro seguro y resistente a reentrancy.
* **Consulta de Balance:** Función para que cualquier usuario pueda consultar su balance actual.
* **Estadísticas del Banco:** Función para consultar el total de depósitos y retiros realizados.

---

## ⚙️ Detalles Técnicos del Contrato

### Constantes e Inmutables

| Nombre | Tipo | Valor Inicial | Descripción |
| :--- | :--- | :--- | :--- |
| `withdrawal_Limit` | `uint256 public immutable` | `1 ether` (1 ETH) | Límite máximo de ETH que se puede retirar por transacción. |
| `bank_Cap` | `uint256 public constant` | `1000 ether` (1000 ETH) | El capital máximo que el contrato puede contener. |

### Almacenamiento

| Nombre | Tipo | Visibilidad | Descripción |
| :--- | :--- | :--- | :--- |
| `_balances` | `mapping (address => uint256) private` | `private` | Almacena el balance de ETH (en Wei) de cada dirección de usuario. |
| `totalDeposits` | `uint256 public` | `public` | Contador del número total de depósitos realizados. |
| `totalWithdrawals` | `uint256 public` | `public` | Contador del número total de retiros realizados. |

### Funciones Clave

| Función | Visibilidad | Modificadores | Descripción |
| :--- | :--- | :--- | :--- |
| `deposit()` | `external payable` | `nonZeroValue` | Permite al usuario enviar ETH al contrato y lo registra en su balance. **Revierte si supera el `bank_Cap`.** |
| `withdraw(uint256 amount)` | `external` | N/A | Permite al usuario retirar `amount` de ETH. **Verifica `withdrawal_Limit` y balance suficiente.** |
| `executeWithdrawal(address user, uint256 amount)` | `private` | N/A | Función interna que realiza la transferencia de ETH (`call`) y actualiza el estado. |
| `getBalance(address user)` | `external view` | N/A | Retorna el balance de ETH (en Wei) de una dirección específica. |
| `getStats()` | `external view` | N/A | Retorna el total de depósitos y retiros realizados. |

---

## 🚫 Errores Personalizados (Custom Errors)

El contrato utiliza los siguientes errores personalizados para una mejor gestión y claridad en los fallos:

* `WithdrawalExceedsLimit(uint256 requested, uint256 limit)`
* `InsufficientBalance(uint256 available, uint256 requested)`
* `DepositExceedCap(uint256 currentTotal, uint256 requested, uint256 cap)`
* `NoBalanceToWithdraw()`
* `TransferFailed()`

---

## 📢 Eventos

Los siguientes eventos son emitidos para indexar y rastrear las transacciones en la cadena:

* `event Deposit (address indexed user, uint256 amountInWei)`
* `event Withdrawal(address indexed user, uint256 amountInWei)`

---

## 🚀 Uso y Despliegue

### Requisitos

* Compilador de Solidity **versión 0.8.0 o superior** (`pragma solidity >0.8.0;`).
* Una billetera de Ethereum con ETH para el despliegue y las interacciones.
* Entorno de desarrollo como **Remix**, **Hardhat**, o **Foundry**.

### Interacción (Ejemplo con Web3/Ethers.js)

#### 1. Depositar

```javascript
// Asegúrate de enviar ETH en el objeto de la transacción
const amountToSend = ethers.utils.parseEther("0.5"); // 0.5 ETH
const tx = await kipuBank.deposit({ value: amountToSend });
await tx.wait();
console.log("Depósito exitoso!");
2. Retirar
JavaScript

// El monto a retirar se pasa como argumento, no en el valor de la transacción.
const amountToWithdraw = ethers.utils.parseEther("0.1"); // 0.1 ETH
const tx = await kipuBank.withdraw(amountToWithdraw);
await tx.wait();
console.log("Retiro exitoso!");
3. Consultar Balance
JavaScript

const userAddress = "0x..."; // Dirección del usuario
const balance = await kipuBank.getBalance(userAddress);
console.log(`Balance del usuario: ${ethers.utils.formatEther(balance)} ETH`);

