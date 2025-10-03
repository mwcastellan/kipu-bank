🏦 KipuBank – Contrato inteligente en Solidity
Autor: Marcelo Walter Castellan 
Fecha: 03/10/2025 
Red: Sepolia Testnet Dirección del contrato: 0xB14350EB8dC6696E5C55de80FFc62D985A9DEeF4

📌 Descripción
KipuBank es un contrato inteligente que simula un banco descentralizado en Ethereum. Cada usuario puede depositar y retirar ETH en su bóveda personal, respetando dos restricciones clave:

Límite global de depósitos (bankCap): impide que el contrato reciba más ETH que el tope definido en el despliegue.

Límite de retiro por transacción (withdrawLimit): restringe cuánto puede retirar un usuario en una sola operación.

El contrato está diseñado con buenas prácticas de seguridad, trazabilidad y eficiencia de gas.

⚙️ Funcionalidades
✅ Depósito
Los usuarios pueden depositar ETH si el total no excede bankCap.

Se valida que el monto sea mayor a cero.

Se actualiza el balance del usuario y se emite un evento.

✅ Retiro
Los usuarios pueden retirar ETH si:

Tienen fondos suficientes.

El monto solicitado no excede withdrawLimit.

Se usa call para transferencias seguras.

Se emite un evento de retiro.

✅ Consulta de balance
Cualquier usuario puede consultar su balance con getBalance(address).

🧱 Estructura del contrato
Componente	Descripción
bankCap	Límite global de depósitos (inmutable)
withdrawLimit	Límite por transacción (inmutable)
_balances	Mapping privado de balances por usuario
totalDeposits	Contador de depósitos
totalWithdrawals	Contador de retiros
Deposit()	Evento emitido al depositar
Withdrawal()	Evento emitido al retirar
DepositExceedCap	Error si el depósito excede el límite global
WithdrawalExceedsLimit	Error si el retiro excede el límite por transacción
InsufficientBalance	Error si el usuario no tiene suficiente balance
NoBalanceToWithdraw	Error si el usuario intenta retirar sin fondos
TransferFailed	Error si la transferencia con call falla
🛡️ Buenas prácticas aplicadas
Uso de immutable para eficiencia de gas.

Validaciones con errores personalizados (error) en lugar de require con strings.

Patrón checks-effects-interactions en retiros.

Transferencias seguras con call.

Eventos para trazabilidad.

Modificador nonZeroValue para evitar depósitos nulos.

🚀 Despliegue
Este contrato fue desplegado en la red Sepolia usando Remix IDE. La dirección del contrato puede consultarse en Etherscan Sepolia: 0xB14350EB8dC6696E5C55de80FFc62D985A9DEeF4


