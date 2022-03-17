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
            "label": "dc app bash",
            "linux": "sudo docker-compose exec app bash",
            "windows": "docker-compose exec app bash",
            "command": "sudo docker-compose exec app bash",
            "type": "shell",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "dedicated",
                "showReuseMessage": false,
                "clear": false
            }
        },
        {
            "label": "dc db bash",
            "command": "sudo docker-compose exec db bash",
            "windows": "docker-compose exec db bash",
            "linux": "sudo docker-compose exec db bash",
            "type": "shell",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "dedicated",
                "showReuseMessage": false,
                "clear": false
            }
        },
        {
            "label": "dc app start",
            "command": "sudo docker-compose up -d app db",
            "linux": "sudo docker-compose up -d app db",
            "windows": "docker-compose up -d app db",
            "type": "shell",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "dedicated",
                "showReuseMessage": false,
                "clear": false
            }
        },
        {
            "label": "docker notice",
            "type": "shell",
            "command": "echo 'There are vs code tasks for this project, opent the command pallet with [ctrl + shift + p > exec task] to see the list of available tasks"
        },
        {
            "label": "dc exec profiler:client",
            "type": "shell",
            "command": "sudo docker-compose up pc",
            "windows": "docker-compose up pc",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "shared",
                "showReuseMessage": true,
                "clear": false
            }
        },
        {
            "label": "dc exec profiler:server",
            "type": "shell",
            "command": "sudo docker-compose exec app php artisan profiler:server",
            "windows": "docker-compose exec app php artisan profiler:server",
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "dedicated",
                "showReuseMessage": true,
                "clear": false
            }
        },
        {
            "label": "dc profiling",
            "dependsOn": ["dc exec profiler:server", "dc exec profiler:client"]
        },

        {
            "label": "dc down",
            "command": "sudo docker-compose down"
        },
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
