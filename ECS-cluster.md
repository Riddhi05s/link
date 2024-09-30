after pushing image to ecr

1. Create one emply ecs cluster
2. Task definitions >> Create new task definition
3. give some Task Definition Family
4. Give some container name and copy image URL
5. Create Task Definition
6. In previously created cluster create new service
7. Give service name
8. In netwroking section of service add 8080 TCP rule
9. Create service
10. One task will be automatically created
11. Check if its state in running
12. Copy ip address and use 8080 port with /webapp path
13. Successfully hosted image on container of ECS.
