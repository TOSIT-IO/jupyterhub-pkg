# Jupyterhub Packing

## Preparation

```bash
npm-pack-all
```

## To create Python dependency archive

The following commands can be launched from the TDP build container:

```bash
JUPYTERHUB=jupyterhub-2.3.1
ARCHIVE=target/$JUPYTERHUB.tar.gz
python3.6 -m pip download --requirement python/requirements.txt --platform manylinux1_x86_64 --only-binary :all: --dest target/$JUPYTERHUB/dependencies
cp python/requirements.txt target/$JUPYTERHUB
tar cf $ARCHIVE -C target $JUPYTERHUB
sha256sum $ARCHIVE | sed 's| .*/| |' > $ARCHIVE.sha256
```
