# Jupyterhub Packing

## To create dependency archive

The following commands can be launched from the TDP build container:

```bash
mkdir target
# Jupyterhub
JUPYTERHUB=jupyterhub-2.3.1
mkdir -p target/$JUPYTERHUB/bin
ARCHIVE=target/$JUPYTERHUB.tar.gz
python3.6 -m pip download --requirement jupyterhub/requirements.txt --platform manylinux1_x86_64 --only-binary :all: --dest target/$JUPYTERHUB/dependencies
cp jupyterhub/requirements.txt target/$JUPYTERHUB
# Traefik
TRAEFIK_VERSION=v1.7.29
TRAEFIK=traefik_linux-amd64
curl -L --output target/$JUPYTERHUB/bin/traefik "https://github.com/traefik/traefik/releases/download/$TRAEFIK_VERSION/$TRAEFIK"
chmod 755 target/$JUPYTERHUB/bin/traefik
# Final Archive
tar cf $ARCHIVE -C target $JUPYTERHUB
sha256sum $ARCHIVE | sed 's| .*/| |' > $ARCHIVE.sha256
```
