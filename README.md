# pipe-to-docker-compose
vs code task to reset a database [drop db, create db, import db]


## How to 

add the task labeled ```dc drop mysql:database``` task to your task entry in .vscode/tasks.json
add the inputs specified into your inputs list in .vscode/tasks.json

fire up your command pallet and try 

{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      
        {
            "label": "dc drop mysql:database",
            "type": "shell",
            "command": "echo \"drop database ${input:database_name};\" | sudo docker-compose exec -T CONTAINER_NAME_OR_ID mysql -u root ; echo \"create database ${input:database_name};\" | sudo docker-compose exec -T CONTAINER_NAME_OR_ID mysql -u root ;echo \"database created; proceeding to import\" ;cat ${input:sqlFullPath} | sudo docker-compose exec -T CONTAINER_NAME_OR_ID mysql -u root laraapp_db; echo \"import complete\""
        }
    ],
    "inputs": [
        <!-- ...// soome existing inputs if any -->
        {
            "id": "sqlFullPath",
            "description": "Enter full path of sql file",
            "type": "promptString",
            "default": "default_file.sql"
        },
        {
            "id": "database_name",
            "description": "Enter the name of the db, check in your .env file",
            "type": "promptString",
            "default": "DEFUALT_DB_NAME"
        }
    ]
}
