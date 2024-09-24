# Architettura bot

<img src="./assets/bots.png" alt="carl">


    graph TD
        A[Restaurant]
        B[User Bot]
        C[Owner Bot]
        D[User Telegram Mini App]
        E[Owner Management Web App]
        F[NestJS Backend]
        G[Supabase Database]
    
        A --> B
        A --> C
        B --> D
        C --> E
        C --> |Receive Orders| F
        D --> |Place Orders| F
        E --> |Manage Menu| F
        F <--> G
    
        style E fill:#f9f,stroke:#333,stroke-width:4px
        style C fill:#bbf,stroke:#333,stroke-width:4px