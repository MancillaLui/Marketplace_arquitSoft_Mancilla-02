# Estilo Arquitectónico

**Selección:** Monolito modular + arquitectura en capas.

Se ha seleccionado un Monolito Modular organizado en capas. Esto permite que los módulos (Usuarios, Sellers, Catálogo, Carrito, Pedidos) mantengan sus responsabilidades separadas de forma lógica (Presentación, Lógica de Negocio, Datos), pero se desplieguen juntos como una única aplicación para facilitar la operación inicial.

## Diagrama de Arquitectura (Capas)

```mermaid
flowchart TD
    %% Actores
    C[Cliente]
    S[Seller]
    A[Administrador]

    %% Cliente Web
    CW["Cliente Web (Navegador HTML/CSS/JS)"]
    
    C --> CW
    S --> CW
    A --> CW

    %% Backend Monolito
    subgraph Backend [Monolito Marketplace Backend Node.js]
        direction TB
        MW[Middlewares Express transversales]
        
        subgraph CapaPresentacion [1. CAPA DE PRESENTACION]
            direction LR
            P_US[modulo usuarios]
            P_SE[modulo sellers]
            P_CA[modulo catalogo]
            P_CR[modulo carrito]
            P_PE[modulo pedidos]
        end
        
        subgraph CapaLogica [2. CAPA DE LOGICA DE NEGOCIO]
            direction LR
            L_US[usuarios.service]
            L_SE[sellers.service]
            L_CA[catalogo.service]
            L_CR[carrito.service]
            L_PE[pedidos.service]
        end
        
        subgraph CapaDatos [3. CAPA DE DATOS]
            direction LR
            D_US[usuarios.repository]
            D_SE[sellers.repository]
            D_CA[catalogo.repository]
            D_CR[carrito.repository]
            D_PE[pedidos.repository]
            
            ORM[Acceso a datos compartido: Sequelize ORM]
        end
        
        %% Conexiones internas
        MW --> P_US & P_SE & P_CA & P_CR & P_PE
        
        P_US --> L_US
        P_SE --> L_SE
        P_CA --> L_CA
        P_CR --> L_CR
        P_PE --> L_PE
        
        L_US --> D_US
        L_SE --> D_SE
        L_CA --> D_CA
        L_CR --> D_CR
        L_PE --> D_PE
        
        D_US & D_SE & D_CA & D_CR & D_PE --> ORM
    end
    
    %% Conexiones externas
    CW -- HTTPS / JSON API REST --> MW
    
    PP[Pasarela de pagos]
    SE[Servicio de envios]
    BD[(PostgreSQL)]
    
    L_PE -- HTTPS / REST --> PP
    L_PE -- HTTPS / REST --> SE
    
    ORM -- SQL TCP 5432 --> BD
    
    %% Estilos
    style CapaPresentacion fill:#e6f3ff,stroke:#4a90e2
    style CapaLogica fill:#e6ffe6,stroke:#5ebd5e
    style CapaDatos fill:#fff2e6,stroke:#ffb366
    style BD fill:#e6e6e6,stroke:#999
```

### Reglas de la arquitectura
1. Cada capa solo conoce a la capa inmediatamente inferior.
2. Un módulo no accede al repository ni a las tablas de otro módulo.
3. La comunicación entre módulos se hace llamando a su service.
4. Todo se ejecuta en un único proceso Node.js con una única BD.