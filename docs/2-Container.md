```plantuml
@startuml
[[!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml]]
!define DEVICONS https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master/devicons

!include DEVICONS/nginx.puml
!include DEVICONS/go.puml
!include DEVICONS/docker.puml



Person(user, "User")
System(app, "Mobile App", "The application interface that the user interacts with contents")

System_Boundary(ext_sys, "External system"){
System_Ext(ext_pay, "Apple | Google \n payment", "Payment services")
System_Ext(ext_anal, "Apple | Google \n analytics", "Analytics services")
}

Lay_L(app, ext_sys)
Rel(user, app, "User", "Hand")
Rel(app, ext_sys, "Metrics", "Analytics & Payment")

Container_Boundary(ctx_sys, "Content system"){
    Container(balancer, "Balancer", "Request balancer", $sprite="nginx")   
    Container_Boundary(apis, "Swarms"){
        AddProperty("MAX-NEW-USERS", "5000000")
        AddProperty("TPS", "3000")
        Container(api, "API", "Handles all content logic, rules etc", $sprite="go")
    }
    ContainerDb(ctx_store, "Store", "Content store")
    ContainerDb(ctx_cache, "Cache", "Content cache")
    ContainerQueue(mq, "Message queue")
}

Rel(app, balancer, "content", "https")
Rel(balancer, api, "content", "https")
Rel(api, ctx_cache, "content", "https")
Rel(api, ctx_store, "content", "https")
Rel_L(api, mq, "Metadata about user contents", "gRPC")

System_Boundary(anal_sys, "Analytic system"){
    Container(ml, "ML", "Prepare personal content use analytics")
}

Rel_R(ext_sys, anal_sys, "App analytics", "https")
Rel_R(mq, anal_sys, "Metadata about user contents", "gRPC")

Container_Boundary(pay_sys, "Payment system"){
    Container(pay_api, "API", "Payment", $sprite="go")
    ContainerDb(pay_db, "Payments db", "Content store")
}

Rel(ext_sys, pay_sys, "Payment", "https")
Rel(pay_api, pay_db, "Payment", "-")
@enduml
```