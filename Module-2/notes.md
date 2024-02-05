# Orchestration

## what and why?

Orchestration in data engineering refers to the coordination and management of various data-related processes and workflows to ensure the seamless and efficient execution of data pipelines.   
so now process of dependency management is facilitated through automation.   

Key elements:
1. Workflow Management --> sequential steps(tasks)
2. Task Dependencies --> DAGs
3. Automation
4. Parallelization
5. Monitoring and Logging
6. Resource Management
7. Error handling
8. recovery
9. observability & debugging
10. compliance & auditing

## what developer experience in orchestration?
1. flow state
2. feedback loops
3. cognitive load

## what is mage?
open source pipeline tool for orchestrating, transforming and integrating data.   
main components:   
1. projects
2. pipelines
3. blocks

blocks: just pieces of code so you can reuse them, copy & paste, and many other things.

## mage install & run
we will use the mage docker image, so first setup your dockerfile like [that](./mage-zoomcamp//Dockerfile) then the compose file like [that](./mage-zoomcamp/docker-compose.yml) then run these commands   
1. `docker compose build`
2. `docker compose up`   
make sure to create you own .env file.

now open any browser to the port 6787 and here you are with mage.