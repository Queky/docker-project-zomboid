Ejecutar uno de los dos:
```bash
docker run -it --name=steamcmd -p 8766:8766/udp -p 16261:16261/udp cm2network/steamcmd bash
docker run -it --name=steamcmd --network host cm2network/steamcmd bash
```

Crear archivo `update_zomboid.txt` con la siguiente conf:
```text
// update_zomboid.txt
//
@ShutdownOnFailedCommand 1 //set to 0 if updating multiple servers at once
@NoPromptForPassword 1
force_install_dir /opt/pzserver/
//for servers which don't need a login
login anonymous 
app_update 380870 validate
quit
EOL
```

Ejecutar `steamcmd` instalando el server para project-zomboid en el contenedor:
```bash
./steamcmd.sh +runscript $HOME/steamcmd/update_zomboid.txt
```

Para arrancar el servidor:
```bash
cd /home/steam/Steam/steamapps/common/Project\ Zomboid\ Dedicated\ Server/
./start-server.sh
```

FIX al iniciar juego (usar garbage collector viejo):
-XX:+UseZGC" to "-XX:+UseG1GC" in ProjectZomboid64.json

Con ese PID podemos matar un proceso que está corriendo dentro del contenedor usando un `kill`
```bash
docker top <container-id>
```
