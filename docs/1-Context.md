```plantuml
@startuml
[[!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml]]

Person(user, "User")
System(app, "Mobile App", "The application interface that the user interacts with contents")
System(ctx, "Contents", "Handles all contents for users")
System(pay, "Payments", "Our payment system")

System_Ext(pay_store, "Apple | Google payments", "External payment system")
System_Ext(analytics, "Apple | Google analytic", "External analytic system")


Rel_D(user, app, "User", "Using")

Rel_D(app, ctx, "Content", "https")
Rel_L(app, analytics, "Analytics", "https")
Rel_R(app, pay_store, "Payment", "https")
Rel_L(ctx, analytics, "Analytics", "https")
Rel_R(pay_store, pay, "Payment", "https")
@enduml
```