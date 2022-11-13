# Jupyterhub Packing

## To create dependency archive

The following commands can be launched from the TDP build container:

```bash
TDP_VERSION=TDP-0.1.0
# Jupyterhub
JUPYTERHUB=jupyterhub-2.3.1-$TDP_VERSION
mkdir -p target/$JUPYTERHUB/bin
ARCHIVE=target/$JUPYTERHUB.tar.gz
python3.6 -m pip download --requirement jupyterhub/requirements.txt --platform manylinux1_x86_64 --only-binary :all: --dest target/$JUPYTERHUB/dependencies
cp jupyterhub/requirements.txt target/$JUPYTERHUB
# Yarn Virtual Environment
mkdir -p target/$JUPYTERHUB/share
python3.6 -m venv target/yarn_environment
source target/yarn_environment/bin/activate
source /usr/local/nvm/nvm.sh
nvm use lts/gallium
python3.6 -m pip install --upgrade pip wheel
python3.6 -m pip install -r yarn_environment/requirements.txt
jupyter labextension install @jupyterlab/hub-extension
jupyter labextension install @jupyter-widgets/jupyterlab-manager
cp -r yarn_environment/kernels/ target/yarn_environment/share/jupyter
venv-pack -o target/$JUPYTERHUB/share/yarn_environment.tar.gz
chmod 644 target/$JUPYTERHUB/share/yarn_environment.tar.gz
deactivate
# Traefik
TRAEFIK_VERSION=v1.7.29
TRAEFIK=traefik_linux-amd64
curl -L --output target/$JUPYTERHUB/bin/traefik "https://github.com/traefik/traefik/releases/download/$TRAEFIK_VERSION/$TRAEFIK"
chmod 755 target/$JUPYTERHUB/bin/traefik
# Final Archive
tar cf $ARCHIVE -C target $JUPYTERHUB
sha256sum $ARCHIVE | sed 's| .*/| |' > $ARCHIVE.sha256
```
