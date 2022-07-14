# Instrucciones de instalación Bundlr

Amigos, hola a todos. Hoy te contaré cómo participar en la prueba de Bundlr. Necesitarás un servidor con estas especificaciones (si es menor, no pasa nada):

- Memoria: 8 GB de RAM
- CPU: Quad-Core
- Disco: 250 GB de almacenamiento SSD
- Ancho de banda: 1 Gbps para descarga/100 Mbps para subida

## Pasos para la instalación

## 1. Establezca las dependencias necesarias para una puesta en marcha exitosa

#### 1.1 Instale y actualice los paquetes necesarios antes de la instalación.
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl ncdu htop git wget build-essential libssl-dev gcc make libssl-dev pkg-config npm -y
```

#### 1.2 Instale de la versión 16.16.0 de nvm
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
source ~/.bashrc
nvm install v16.16.0
nvm use v16.16.0
nvm alias default v16.16.0
node --version
```

#### 1.3 Instale Docker en nuestro servidor para seguir ejecutando contenedores
```
cd ~
apt update && apt purge docker docker-engine docker.io containerd docker-compose -y
rm /usr/bin/docker-compose /usr/local/bin/docker-compose
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
curl -SL https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

## 2. Copiar el contenedor con los archivos de origen a nuestro servidor
```
git clone --recurse-submodules https://github.com/Bundlr-Network/validator-rust.git
```

## 3. Genera la dirección en la blockchain de Arweave que necesitaremos para probar Bundlr
```
cd validator-rust
cargo run --bin wallet-tool create > wallet.json
```
Debería ver un archivo wallet.json, que debería estar ubicado en `/root/validator-rust` .
Asegúrese de guardar el archivo para usted, ya que es la clave de su dirección. 

## 4. Cambiar las dependencias para conectarse con éxito al validador

#### 4.1 Busque el archivo example.env y cambie los datos dentro de él
```
PORT=80
BUNDLER_URL=https://testnet1.bundlr.network
GW_CONTRACT=”RkinCLBlY4L5GZFv8gCFcrygTyd5Xm91CzKlR6qxhKA”
GW_ARWEAVE=https://arweave.testnet1.bundlr.network/
```

#### 4.2 ambiar el nombre del archivo para que el sistema lo reconozca
```
cp example.env .env
```

## 5. Inicie nuestro contenedor y compruebe los registros
```
cd validator-rust
docker-compose up -d
docker-compose logs --tail=100 -f
```

## 6. Conexión a una red de prueba
```
npm i -g @bundlr-network/testnet-cli
testnet-cli join RkinCLBlY4L5GZFv8gCFcrygTyd5Xm91CzKlR6qxhKA -w wallet.json -u "http://Introduzca aquí la IP de su servidor:80)" -s 25000000000000
```

## A continuación, recibirá la esperada respuesta "Done"
