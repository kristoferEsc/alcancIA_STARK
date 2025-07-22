# Información de Deployment - Starknet Sepolia

## Contratos Desplegados

### 1. YieldManager
- **Class Hash**: `0x06653db9d1602a89f178424256d2873ee316e2d597fa6596cebeac97c80e1dfe`
- **Contract Address**: `0x063626cdc8af81455e0d0e86aee5fdc7df511571273a71c7d7603877d4c91992`
- **Transaction Hash**: `0x0693416b01619798a172dd0339892ac7b98b6890fefaf8eb88464f77bf47ee21`
- **Admin**: `0x467402f63069d85c6b7eaf545e0d0df9a8a2b1d507e993b32beeeb39040bdd8`
- **Explorer**: https://sepolia.starkscan.co/contract/0x063626cdc8af81455e0d0e86aee5fdc7df511571273a71c7d7603877d4c91992

### 2. GroupSavings
- **Class Hash**: `0x02d36efd87a6f0d631234b55b0c371e81bc77be8be1f3c383a9c1a7e3f83fa77`
- **Contract Address**: `0x02de63b6c85a125846792d0708dd91ee1d3e32d8e054660fe8d59ca47382cea6`
- **Transaction Hash**: `0x065a04519a5613066332a96dd85a65f99baa80ec847708e602ea2dba61234e30`
- **Explorer**: https://sepolia.starkscan.co/contract/0x02de63b6c85a125846792d0708dd91ee1d3e32d8e054660fe8d59ca47382cea6

### 3. IndividualSavings
- **Class Hash**: `0x00fb57a8b15bfb7720c48b65a17ca44e70e404b16b9acec1541f82b7354969a2`
- **Contract Address**: `0x07573773a61f52f47a5614dcdc11a557f816459c5304635a9a9e4782b3dcb2b7`
- **Transaction Hash**: `0x0110b18d76d8370b0b599b6b246aea4f7065a7fad4a2b5eeb7924ae53fad8376`
- **Owner**: `0x467402f63069d85c6b7eaf545e0d0df9a8a2b1d507e993b32beeeb39040bdd8`
- **Explorer**: https://sepolia.starkscan.co/contract/0x07573773a61f52f47a5614dcdc11a557f816459c5304635a9a9e4782b3dcb2b7

## Cuenta Utilizada
- **Account**: `stark`
- **Address**: `0x467402f63069d85c6b7eaf545e0d0df9a8a2b1d507e993b32beeeb39040bdd8`
- **Network**: Sepolia Testnet

## Configuración
- **RPC URL**: `https://starknet-sepolia.public.blastapi.io/rpc/v0_8`
- **Block Explorer**: StarkScan
- **Network**: Sepolia Testnet

## Funciones Principales

### YieldManager
- `deposit(from, user, amount)` - Depositar fondos para un usuario
- `distribute_yield()` - Distribuir yield (5%) a todos los usuarios
- `get_user_balance(user)` - Obtener balance de un usuario
- `get_user_yield(user)` - Obtener yield acumulado de un usuario
- `set_authorized_caller(contract, is_auth)` - Autorizar/desautorizar contratos
- `set_penalty(user, amount)` - Establecer penalización para un usuario
- `set_bonus(user, amount)` - Establecer bono para un usuario

### GroupSavings
- `register_group(group_id, name, members)` - Registrar un nuevo grupo
- `save(group_id, member, amount)` - Ahorrar para un miembro del grupo
- `get_group_total(group_id)` - Obtener total del grupo
- `get_member_savings(group_id, member)` - Obtener ahorros de un miembro
- `get_group_member(group_id, index)` - Obtener miembro por índice
- `get_group_size(group_id)` - Obtener tamaño del grupo

### IndividualSavings
- `create_savings_goal(goal_id, target_amount, deadline, description)` - Crear una meta de ahorro
- `deposit(goal_id, amount)` - Depositar a una meta
- `withdraw(goal_id, amount)` - Retirar de una meta
- `complete_goal(goal_id)` - Completar una meta manualmente
- `get_goal_info(goal_id)` - Obtener información completa de una meta
- `get_goal_progress(goal_id)` - Progreso de una meta
- `get_user_goals(user)` - Listar metas de un usuario
- `get_goal_count(user)` - Número de metas de un usuario
- `apply_penalty(goal_id, penalty_amount)` - Aplicar penalización a una meta
- `apply_bonus(goal_id, bonus_amount)` - Aplicar bonificación a una meta
- `get_penalties(goal_id)` - Consultar penalizaciones
- `get_bonuses(goal_id)` - Consultar bonificaciones
- `set_owner(new_owner)` - Cambiar owner del contrato
- `get_owner()` - Consultar owner actual

## Notas Importantes
- Los contratos están desplegados en **Sepolia Testnet**
- Para producción, desplegar en **Mainnet**
- Los asserts de admin están comentados para testing
- Restaurar los asserts de admin para producción 