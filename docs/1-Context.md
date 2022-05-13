```plantuml
@startuml
[[!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml]]

LAYOUT_TOP_DOWN()

Person(user, "User")
System(app, "Mobile App", "The application interface that the user interacts with contents")
System(ctx, "Contents", "Handles all contents for users")
System(pay, "Payments", "Our payment system")

System_Ext(pay_store, "Apple | Google payments", "External payment system")
System_Ext(analytics, "Apple | Google analytic", "External analytic system")


Rel(user, app, "User", "Using")

Rel(app, ctx, "Content", "https")
Rel(app, analytics, "Analytics", "https")
Rel(app, pay_store, "Payment", "https")
Rel(ctx, analytics, "Analytics", "https")
Rel(pay_store, pay, "Payment", "https")

@enduml
```