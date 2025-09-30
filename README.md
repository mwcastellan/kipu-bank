# kipu-bank

🏦 KipuBankNotFinished
KipuBankNotFinished es un contrato inteligente en Solidity que simula una billetera bancaria básica donde los usuarios pueden depositar y retirar fondos. Aún está en desarrollo, pero ya incluye mecanismos de seguridad como protección contra reentrancia y múltiples métodos de retiro.

📜 Licencia
Este contrato está bajo la licencia MIT.

solidity
// SPDX-License-Identifier: MIT
⚙️ Versión de Solidity
solidity
pragma solidity >0.8.0;
🚀 Funcionalidades

1. pay()
   Permite a los usuarios depositar exactamente 0.1 ether en el contrato.

Verifica que el monto sea correcto.

Registra el depósito en el mapping balance.

Guarda la dirección en el array addr.

Emite el evento paid.

solidity
function pay() external payable 2. withdraw()
Retira el saldo del usuario usando call, con protección contra reentrancia.

Usa el modificador reentrancyGuard.

Establece el balance en cero antes de transferir.

Devuelve los datos de la llamada.

solidity
function withdraw() external reentrancyGuard returns(bytes memory) 3. withdraw2()
Retira el saldo usando .transfer().

Establece el balance en cero antes de transferir.

Usa gas limitado (2300), lo que puede fallar si el receptor tiene lógica compleja.

solidity
function withdraw2() external 4. withdraw3()
Retira el saldo usando .send().

Similar a .transfer(), pero devuelve un booleano.

Revierte si la transferencia falla.

solidity
function withdraw3() external
🛡️ Seguridad
Protección contra reentrancia: Implementada con el modificador reentrancyGuard usando una bandera booleana.

Validación de valor: Solo se acepta exactamente 0.1 ether por depósito.

Balance cero antes de transferir: Previene ataques de reentrancia.

📦 Variables clave
Nombre Tipo Descripción
balance mapping(address => uint256) Registra el saldo de cada usuario. No iterable.
addr address[] Lista de direcciones que han pagado.
flag bool Usada para bloquear reentrancia.
📣 Eventos
solidity
event paid(address indexed payer, uint256 amount);
Se emite cada vez que un usuario realiza un pago exitoso.

⚠️ Consideraciones
El contrato no tiene funciones administrativas ni de recuperación.

addr[] puede crecer indefinidamente, lo que podría afectar el gas en futuras funciones.

No hay validación para evitar múltiples entradas duplicadas en addr.
