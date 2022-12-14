

### Intalaciones
```bash
npm i express cors dotenv multer mongoose nodemon
```
- _express_: Libreria principal
- _cors_: Libreria para poder consumir el endpoint desde diferentes origenes
- _dotenv_: Libreria de variables de entorno
- _multer_: Cargar archivos a la app
- _mongoose_: para conexion de bd con mongo

#### __Tipados__
Los paquetes de tipados ayudan a que la app interprete de la mejor manera el typescript.
```bash
npm i @types/express @types/cors @types/dotenv @types/multer @types/mongoose -D
```

#### __Implemetanción MVC__

### _Cargador dinamico de rutas_
```ts
import { Router } from "express";
import { readdirSync } from 'fs';

const PATH_ROUTER = `${__dirname}`;
const router = Router();

/**
 * 
 * @param fileName string
 */
const cleanFileName = (fileName: string) => {
    const file = fileName.split('.').shift();
    return file;
}

readdirSync(PATH_ROUTER).filter((fileName) => {
    const cleanName = cleanFileName(fileName);

    if( cleanName !== 'index' ) {
        import(`./${cleanName}`).then((moduleRouter) => {
            console.log(`Se esta cargando la ruta... /${cleanName}`);            
            router.use(`/${cleanName}`, moduleRouter.router);
        });        
    }

});

export { router };
```
