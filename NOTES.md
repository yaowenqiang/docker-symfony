> docker run -d --rm ubuntu bash -c "while true; do echo \$RANDOM; sleep 1; done"

> docker run -d --name="logspout" \
       --volume=/var/run/docker.sock:/var/run/docker.sock \
       --publish=127.0.0.1:8000:80 \
       gliderlabs/logspout

> curl http://127.0.0.1:8000/logs
> curl http://127.0.0.1:8000/logs/name:container_name
> curl http://127.0.0.1:8000/logs/filter:e

> https://github.com/gliderlabs/logspout

# jenkinsfile syntax check
> curl --user admin:123456 -X POST -F "jenkinsfile=<Jenkinsfile" http://192.168.101.18:8080/pipeline-model-converter/validate
