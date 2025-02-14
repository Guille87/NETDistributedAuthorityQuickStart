## Scripts

### ConnectionManager.cs
- **Variables**:
    - `_profileName` → Nombre del perfil del jugador.
    - `_sessionName` → Nombre de la sesión de red.
    - `_maxPlayers` → Número máximo de jugadores en la sesión.
    - `_state` → Estado de la conexión.
    - `_session` → Instancia de la sesión de red.
    - `m_NetworkManager` → Referencia al componente `NetworkManager`.
    - `ConnectionState` → Define los posibles estados de la conexión.
- **Métodos**:
    - `Awake()` → Obtiene el `NetworkManager` del GameObject. Se suscribe a los evento de conexión y del dueño de la sesión. Inicializa los servicios de Unity.
    - `OnSessionOwnerPromoted(ulong sessionOwnerPromoted)` → Se activa cuando cambia el dueño de la sesión. Si el cliente local es dueño, muestra un mensaje en consola.
    - `OnClientConnectedCallback(ulong clientId)` → Se ejecuta cuando un cliente se conecta al servidor. Si el cliente local es el que se conectó, muestra un mensaje indicando que puede generar objetos en red.
    - `OnGUI()` → Dibuja la interfaz gráfica. Muestra los respectivos botones para crear o unirse a una sesión. Llama al método `CreateOrJoinSessionAsync()` cuando se presiona el botón.
    - `OnDestroy()` → Se ejecuta al destruir el objeto. Si hay una sesión activa, intenta abandonarla.
    - `CreateOrJoinSessionAsync()` → Cambia la variable `_state` a **Conectando**. Intenta autenticar al usuario anónimamente con **Unity Authentication**. Crea o se une a una sesión usando **Unity Multiplayer Service** con **Distributed Authority Network**. Si la conexión es exitosa, cambia el estado a **Conectado**. Si hay un error, vuelve al estado **Desconectado** y lo muestra en la consola.

### PlayerCubeController.cs
- **Variables**:
    - `Speed` → Controla la velocidad de movimiento del jugador.
    - `ApplyVerticalInputToZAxis` → Determina si el input vertical afecta el eje Z en lugar del eje Y.
    - `m_Motion` → Almacena el movimiento basado en la entrada del usuario.
    - `PlayerCubeControllerPropertiesVisible` → Guarda el estado de visibilidad de las propiedades en el **Inspector** de Unity.

- **Métodos**:
    - `Update()` → Solo se ejecuta si el objeto ha sido **spawneado** y tiene **autoridad**. Maneja la entrada del usuario. Determina si la entrada vertical se aplica al eje Y o Z. Si hay movimiento, entonces aplica esa cantidad de movimiento al transform.

### Editor Personalizado (`PlayerCubeControllerEditor`)
- **Variables**:
    - `m_Speed` → Referencia a la variable `Speed` para editarla en el **Inspector**.
    - `m_ApplyVerticalInputToZAxis` → Referencia a la variable `ApplyVerticalInputToZAxis` para editarla en el **Inspector**.
- **Métodos**:
    - `OnEnable()` → Obtiene las referencias a `Speed` y `ApplyVerticalInputToZAxis` al abrir el Inspector.
    - `DisplayPlayerCubeControllerProperties()` → Muestra los campos `Speed` y `ApplyVerticalInputToZAxis` en el Inspector.
    - `OnInspectorGUI()` → Agrega una seeción desplegable en el Inspector para mostrar/ocultar las propiedades del `PlayerCubeController`.

## Componentes del GameObject `NetworkManager`
### Network Manager
- Gestiona la comunicación entre clientes y el servidor.
- Sincroniza objetos en la red.
### Unity Transport
- Transporta datos para enviar y recibir paquetes de red.
### Connection Manager
- Gestiona la **conexión** y **sesiones multijugador**. Administra el estado de la conexión. Permite a los jugadores **crear o unirse a una sesión**. Maneja la lógica de **autenticación** y **sesión** en red.

## Componentes del Prefab `PlayerCube`
### Network Object
- Permite que el objeto exista en la red y se sincronice entre jugadores. El **Ownership** está en **None**, lo que significa que **no hay un dueño fijo** y la lógica de autoridad se maneja en otro lado.
### Player Cube Controller
- Controla el **movimiento** del **jugador**. Permite la sincronización de posición en red. Maneja el **input del jugador**. Define si el eje **vertical** afecta al **eje** Y o Z.