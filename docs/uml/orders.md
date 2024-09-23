# Sequence diagram plant uml

![carl](/assets/orders.png)

@startuml
title Sequence Diagram: Order Process

actor User
participant TMA as Telegram Mini App
participant BE as Backend (NestJS)
participant DB as Database (Supabase)
participant RB as Restaurant Bot

box "Authentication"
  User->>TMA: Open Mini App
  TMA->>BE: Get user identity (Telegram API)
  BE->>DB: Verify/Create user
  DB-->>BE: User data
  BE-->>TMA: User authenticated
end box

box "Order Creation"
  User->>TMA: Choose order type (Delivery/Takeaway/Table)
  User->>TMA: Browse menu and select items
  User->>TMA: Add customizations/toppings
  User->>TMA: Review order
end box

box "Payment"
  User->>TMA: Initiate payment
  TMA->>BE: Process payment
  BE-->>TMA: Payment confirmation
end box

box "Order Submission"
  TMA->>BE: Submit order
  BE->>DB: Save order
  DB-->>BE: Order saved
  BE->>RB: Send order notification
  RB-->>User: Order confirmation (Telegram message)
end box

box "Order Success"
  BE-->>TMA: Order success
  TMA-->>User: Display order confirmation
end box

@enduml