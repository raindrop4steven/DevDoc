## Flask admin confilict with blueprint

    The collision is because you have a module name user and a blueprint called user. Rename the blueprint to user_blueprint. From the code it seems you have a folder called user, a module called user and a blueprint called user. You can avoid problems later on with some descriptive names. Otherwise it is just plain confusing.