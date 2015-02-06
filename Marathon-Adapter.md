**The Marathon adapter, in combination with the Panamax remote agent, enables the deployment of a Panamax template to a Mesosphere cluster.**

## Template Translation

To understand how the Marathon Adapter translates a Panamax Template into a deployment, let's look at an example template:

    name: Wordpress with MySQL
    images:
    - name: WP
      source: centurylink/wordpress:3.9.1
      environment:
        - variable: DB_PASSWORD
          value: pass@word01
        - variable: DB_NAME
          value: wordpress
      links:
      - service: DB
        alias: DB_1
      ports:
      - host_port: 8000
        container_port: 80
    - name: DB
      source: centurylink/mysql:5.5
      environment:
        - variable: MYSQL_ROOT_PASSWORD
          value: pass@word01
      ports:
      - host_port: 3306
        container_port: 3306
