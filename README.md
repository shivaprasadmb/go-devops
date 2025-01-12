# own readme file, initial changes

buildsafe tool builds 0 CVE base images
bsf init
-> do you want to build a base image => yes
-> what should the image name be => "container-resitry"/"image"
-> do you want to add common dependencies (compiler, linter, debugger etc) => yes
-> which common deps would you like to add (go/python/rust/jsNpm) => go

this creates 1 folder, 2 files => bsf/, bsf.hcl, bsf.lock
bsf uses nix underhood, nix is a cross-platform build manager and ensures exact binary file while copying software from one machine to another

bsf.hcl file shows what dependencies would be combined within the base image

commands:
bsf oci pkgs --platform=linux/amd64 --tag=base --push --dest-creds "username":"password"
-> now the artifact (base image) will be created and pushed to respective registry

ko tool helps in building go specific app images easy, fast and secure by default
ko can also bundle static assets into the images it produces
ref - https://ko.build/features/static-assets/

commands:
ko login docker.io -u "user" -p "password"
KO_DOCKER_REPO="registry" KO_DEFAULTBASEIMAGE="base image built prviously" ko build --bare -t v1

install grype to scan vulnerabilities of built image
curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sudo sh -s -- -b /usr/l
ocal/bin

grype "image" // make sure container runtime is running

running app locally:
- podman run -d --name grafana -p 3000:3000 grafana/grafana
- podman run -d --name prometheus -p 9090:9090 -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
- podman run --name local-postgres -e POSTGRES_USER="" -e POSTGRES_PASSWORD="" -e POSTGRES_DB="" -p 5432:5432 -d postgres
- podman exec -it local-postgres psql -U "" -d mydb
CREATE TABLE goals (
    id SERIAL PRIMARY KEY,
    goal_name TEXT NOT NULL
);
- podmand run -d \
- --platform=linux/amd64 \
- -e DB_USERNAME="" \
- -e DB_PASSWORD="" \
- -e DB_HOST=host.docker.internal \
- -e DB_PORT=5432 \
- -e DB_NAME=mydb \
- -e SSL=disable \
- "go-app-image"

login to grafana and setup datasource as prometheus (host.docker.internal:9090)
- create dashboard, add visualization, select data source (prometheus)
- verify if the data is coming from app