##Construir imagen Docker:
docker build -t hola-mundo-node:latest .

##Ejecutar:
docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest

##Instalar dependencias y ejecutar tests:
npm init -y
npm install express supertest jest
npm install
npm test

##Jenkins:
1. Configura en Jenkins una instalación de NodeJS llamada `Node18`.
2. Verifica que Docker esté disponible en el agente.
3. Crea un Pipeline desde este repositorio usando el archivo `Jenkinsfile`.
4. El pipeline ejecuta `npm install`, `npm test`, construye la imagen Docker y levanta el contenedor.