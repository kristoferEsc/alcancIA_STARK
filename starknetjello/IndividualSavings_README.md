# IndividualSavings Contract

## Descripción

El contrato `IndividualSavings` es un smart contract en Cairo 1.x para Starknet que gestiona la lógica de ahorro individual. Permite a los usuarios crear metas de ahorro, realizar depósitos y retiros, y aplicar penalizaciones o bonificaciones según reglas definidas.

## Características Principales

### 🎯 Gestión de Metas de Ahorro
- **Creación de metas**: Los usuarios pueden definir metas con monto objetivo, plazo y descripción
- **Seguimiento de progreso**: Monitoreo automático del progreso hacia la meta
- **Completado automático**: Las metas se marcan como completadas cuando se alcanza el objetivo

### 💰 Operaciones Financieras
- **Depósitos**: Los usuarios pueden depositar fondos en sus metas
- **Retiros**: Permite retirar fondos de metas no completadas
- **Validación de fondos**: Verifica que haya suficientes fondos para retiros

### ⚖️ Sistema de Penalizaciones y Bonificaciones
- **Penalizaciones**: Aplicación de penalizaciones por incumplimiento de reglas
- **Bonificaciones**: Otorgamiento de bonificaciones por logros
- **Seguimiento**: Registro y consulta de penalizaciones y bonificaciones

### 🔐 Seguridad y Control de Acceso
- **Wallet invisible**: Solo el owner (wallet invisible) puede operar
- **Validación de permisos**: Verificación de propiedad de metas
- **Prevención de duplicados**: Evita la creación de metas duplicadas

## Estructura del Contrato

### Storage

```cairo
#[storage]
struct Storage {
    // Información de metas de ahorro
    goal_owners: Map<felt252, ContractAddress>,
    goal_targets: Map<felt252, u256>,
    goal_deadlines: Map<felt252, u64>,
    goal_descriptions: Map<felt252, felt252>,
    goal_current_amounts: Map<felt252, u256>,
    goal_created_at: Map<felt252, u64>,
    goal_completed: Map<felt252, bool>,
    
    // Penalizaciones y bonificaciones
    goal_penalties: Map<felt252, u256>,
    goal_bonuses: Map<felt252, u256>,
    
    // Gestión de usuarios
    user_goals: Map<(felt252, u32), felt252>,
    user_goal_counts: Map<felt252, u32>,
    
    // Owner del contrato
    owner: Map<(), ContractAddress>,
}
```

### Eventos

El contrato emite los siguientes eventos para tracking:

- `GoalCreated`: Cuando se crea una nueva meta
- `Deposit`: Cuando se realiza un depósito
- `Withdrawal`: Cuando se realiza un retiro
- `GoalCompleted`: Cuando se completa una meta
- `PenaltyApplied`: Cuando se aplica una penalización
- `BonusApplied`: Cuando se aplica una bonificación
- `ProgressUpdated`: Cuando se actualiza el progreso

## Funciones Principales

### Creación y Gestión de Metas

#### `create_savings_goal`
```cairo
fn create_savings_goal(
    ref self: TContractState,
    goal_id: felt252,
    target_amount: u256,
    deadline: u64,
    description: felt252
);
```
- **Propósito**: Crea una nueva meta de ahorro
- **Parámetros**:
  - `goal_id`: Identificador único de la meta
  - `target_amount`: Monto objetivo a alcanzar
  - `deadline`: Fecha límite para completar la meta
  - `description`: Descripción de la meta
- **Validaciones**:
  - Solo el owner puede crear metas
  - La meta no debe existir previamente
  - El monto objetivo debe ser mayor a 0
  - La fecha límite debe ser futura

#### `deposit`
```cairo
fn deposit(ref self: TContractState, goal_id: felt252, amount: u256);
```
- **Propósito**: Realiza un depósito en una meta
- **Validaciones**:
  - Solo el owner de la meta puede depositar
  - La meta debe existir y no estar completada
  - El monto debe ser mayor a 0
- **Funcionalidad**:
  - Actualiza el monto actual de la meta
  - Emite eventos de depósito y progreso
  - Marca la meta como completada si se alcanza el objetivo

#### `withdraw`
```cairo
fn withdraw(ref self: TContractState, goal_id: felt252, amount: u256);
```
- **Propósito**: Retira fondos de una meta
- **Validaciones**:
  - Solo el owner de la meta puede retirar
  - La meta debe existir y no estar completada
  - Debe haber suficientes fondos
- **Funcionalidad**:
  - Actualiza el monto actual de la meta
  - Emite eventos de retiro y progreso

#### `complete_goal`
```cairo
fn complete_goal(ref self: TContractState, goal_id: felt252);
```
- **Propósito**: Marca una meta como completada manualmente
- **Validaciones**:
  - Solo el owner de la meta puede completarla
  - La meta debe existir y no estar completada
  - Se debe haber alcanzado el objetivo

### Consultas

#### `get_goal_info`
```cairo
fn get_goal_info(self: @TContractState, goal_id: felt252) -> (felt252, u256, u64, felt252, u256, u64, bool);
```
- **Retorna**: (owner, target_amount, deadline, description, current_amount, created_at, is_completed)

#### `get_goal_progress`
```cairo
fn get_goal_progress(self: @TContractState, goal_id: felt252) -> (u256, u256, u64);
```
- **Retorna**: (current_amount, target_amount, deadline)

#### `get_user_goals`
```cairo
fn get_user_goals(self: @TContractState, user: felt252) -> Array<felt252>;
```
- **Retorna**: Array con los IDs de todas las metas del usuario

#### `get_goal_count`
```cairo
fn get_goal_count(self: @TContractState, user: felt252) -> u32;
```
- **Retorna**: Número total de metas del usuario

### Penalizaciones y Bonificaciones

#### `apply_penalty`
```cairo
fn apply_penalty(ref self: TContractState, goal_id: felt252, penalty_amount: u256);
```
- **Propósito**: Aplica una penalización a una meta
- **Validaciones**: Solo el owner de la meta puede aplicar penalizaciones

#### `apply_bonus`
```cairo
fn apply_bonus(ref self: TContractState, goal_id: felt252, bonus_amount: u256);
```
- **Propósito**: Aplica una bonificación a una meta
- **Validaciones**: Solo el owner de la meta puede aplicar bonificaciones

#### `get_penalties` / `get_bonuses`
```cairo
fn get_penalties(self: @TContractState, goal_id: felt252) -> u256;
fn get_bonuses(self: @TContractState, goal_id: felt252) -> u256;
```
- **Propósito**: Consulta las penalizaciones o bonificaciones acumuladas

### Funciones Administrativas

#### `set_owner` / `get_owner`
```cairo
fn set_owner(ref self: TContractState, new_owner: ContractAddress);
fn get_owner(self: @TContractState) -> ContractAddress;
```
- **Propósito**: Gestión del owner del contrato (wallet invisible)

## Integración con Wallets Invisibles

El contrato está diseñado para trabajar con wallets invisibles:

1. **Owner del contrato**: El owner es la wallet invisible del usuario
2. **Control de acceso**: Solo la wallet invisible puede operar el contrato
3. **Gestión de metas**: Cada meta pertenece a una wallet invisible específica
4. **Seguridad**: Validaciones estrictas para prevenir acceso no autorizado

## Casos de Uso

### 1. Creación de Meta de Ahorro
```cairo
// Usuario crea una meta para ahorrar 1000 tokens en 30 días
create_savings_goal(
    goal_id: "vacation_2024",
    target_amount: 1000000000000000000000, // 1000 tokens (18 decimales)
    deadline: 1735689600, // 30 días desde ahora
    description: "Vacation savings 2024"
);
```

### 2. Depósito Regular
```cairo
// Usuario deposita 100 tokens
deposit(
    goal_id: "vacation_2024",
    amount: 100000000000000000000 // 100 tokens
);
```

### 3. Aplicación de Penalización
```cairo
// Aplicar penalización por retiro temprano
apply_penalty(
    goal_id: "vacation_2024",
    penalty_amount: 50000000000000000000 // 50 tokens
);
```

### 4. Consulta de Progreso
```cairo
// Obtener información del progreso
let (current, target, deadline) = get_goal_progress("vacation_2024");
```

## Consideraciones de Seguridad

1. **Validación de permisos**: Todas las operaciones verifican que el caller sea el owner
2. **Prevención de overflow**: Uso de tipos u256 para manejar grandes cantidades
3. **Validación de datos**: Verificación de montos positivos y fechas válidas
4. **Estado consistente**: Las operaciones mantienen la consistencia del estado
5. **Eventos de auditoría**: Todos los cambios importantes emiten eventos

## Compilación y Despliegue

### Compilación
```bash
scarb build
```

### Despliegue
```bash
# El contrato se despliega con el owner como la wallet invisible del usuario
starknet deploy --contract target/dev/starknetjello_individualsavings.sierra.json --inputs <owner_address>
```

## Testing

El contrato incluye configuraciones especiales para testing:
- Funciones con `#[cfg(test)]` para facilitar las pruebas
- Validaciones de permisos deshabilitadas en modo test
- Eventos para verificar el comportamiento esperado

## Dependencias

- **starknet**: Framework principal para contratos en Starknet
- **core::integer**: Tipos de datos para enteros sin signo
- **core::array**: Funcionalidades para arrays
- **core::option**: Manejo de valores opcionales

## Licencia

MIT License - Ver archivo LICENSE para más detalles. 