Frontend => npm run build=>  create the dist folder => copy the dist folder to Backend/public=>
use app.use(express.static("public")) in server.js => run the backend server => open the browser and go to http://localhost:3000

![alt text](image.png)
docker build . -t backend

docker run backend
docker run -p 4000:3000 backend