/mi-tienda
│
├── /public
│   ├── index.html
│   ├── style.css
│   └── script.js
│
├── /server
│   └── app.js
└── package.json<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Tienda Online</title>
    <link rel="stylesheet" href="/public/style.css">
</head>
<body>
    <header>
        <h1>Bienvenido a Mi Tienda</h1>
        <nav>
            <a href="#">Inicio</a>
            <a href="#productos">Productos</a>
            <a href="#">Contacto</a>
        </nav>
    </header>

    <section id="productos">
        <h2>Productos Destacados</h2>
        <div class="producto">
            <h3>Curso de Trading</h3>
            <p>$99 USD</p>
            <button onclick="irAPagar('Curso de Trading', 99)">Comprar</button>
        </div>
        <div class="producto">
            <h3>Libro de Trading</h3>
            <p>$20 USD</p>
            <button onclick="irAPagar('Libro de Trading', 20)">Comprar</button>
        </div>
    </section>

    <footer>
        <p>&copy; 2024 Mi Tienda Online</p>
    </footer>

    <script src="/public/script.js"></script>
</body>
</html>body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #333;
    color: white;
    padding: 10px;
    text-align: center;
}

nav a {
    color: white;
    margin: 10px;
    text-decoration: none;
}

#productos {
    padding: 20px;
    display: flex;
    justify-content: center;
    gap: 20px;
}

.producto {
    background-color: white;
    padding: 10px;
    border: 1px solid #ddd;
    text-align: center;
    width: 200px;
}

button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}function irAPagar(nombreProducto, precio) {
    window.location.href = `/comprar?producto=${nombreProducto}&precio=${precio}`;
}npm init -y
npm install express stripe body-parserconst express = require('express');
const bodyParser = require('body-parser');
const stripe = require('stripe')('sk_test_yourStripeSecretKey'); // Reemplaza con tu clave secreta de Stripe

const app = express();
const port = 3000;

// Middleware para procesar datos del formulario
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Servir archivos estáticos (HTML, CSS, JS)
app.use(express.static('public'));

// Página de inicio
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/public/index.html');
});

// Ruta para procesar el pago
app.get('/comprar', (req, res) => {
    const { producto, precio } = req.query;

    res.send(`
        <html>
            <body>
                <h1>Compra: ${producto}</h1>
                <p>Precio: $${precio} USD</p>
                <form action="/procesar-pago" method="POST">
                    <input type="hidden" name="producto" value="${producto}">
                    <input type="hidden" name="precio" value="${precio}">
                    <button type="submit">Pagar con tarjeta</button>
                </form>
            </body>
        </html>
    `);
});

// Ruta para procesar el pago con Stripe
app.post('/procesar-pago', async (req, res) => {
    const { producto, precio } = req.body;

    try {
        const paymentIntent = await stripe.paymentIntents.create({
            amount: precio * 100, // El precio debe estar en centavos
            currency: 'usd',
            description: producto
        });

        res.send(`
            <html>
                <body>
                    <h2>Pago exitoso</h2>
                    <p>Has pagado $${precio} USD por ${producto}</p>
                </body>
            </html>
        `);
    } catch (error) {
        console.error(error);
        res.send('Error al procesar el pago.');
    }
});

app.listen(port, () => {
    console.log(`Servidor escuchando en http://localhost:${port}`);
});/mi-tienda
│
├── /public
│   ├── index.html
│   ├── style.css
│   └── script.js
│
├── /server
│   └── app.js
└── package.json<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Tienda Online</title>
    <link rel="stylesheet" href="/public/style.css">
</head>
<body>
    <header>
        <h1>Bienvenido a Mi Tienda</h1>
        <nav>
            <a href="#">Inicio</a>
            <a href="#productos">Productos</a>
            <a href="#">Contacto</a>
        </nav>
    </header>

    <section id="productos">
        <h2>Productos Destacados</h2>
        <div class="producto">
            <h3>Curso de Trading</h3>
            <p>$99 USD</p>
            <button onclick="irAPagar('Curso de Trading', 99)">Comprar</button>
        </div>
        <div class="producto">
            <h3>Libro de Trading</h3>
            <p>$20 USD</p>
            <button onclick="irAPagar('Libro de Trading', 20)">Comprar</button>
        </div>
    </section>

    <footer>
        <p>&copy; 2024 Mi Tienda Online</p>
    </footer>

    <script src="/public/script.js"></script>
</body>
</html>body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #333;
    color: white;
    padding: 10px;
    text-align: center;
}

nav a {
    color: white;
    margin: 10px;
    text-decoration: none;
}

#productos {
    padding: 20px;
    display: flex;
    justify-content: center;
    gap: 20px;
}

.producto {
    background-color: white;
    padding: 10px;
    border: 1px solid #ddd;
    text-align: center;
    width: 200px;
}

button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}function irAPagar(nombreProducto, precio) {
    window.location.href = `/comprar?producto=${nombreProducto}&precio=${precio}`;
}npm init -y
npm install express stripe body-parserconst express = require('express');
const bodyParser = require('body-parser');
const stripe = require('stripe')('sk_test_yourStripeSecretKey'); // Reemplaza con tu clave secreta de Stripe

const app = express();
const port = 3000;

// Middleware para procesar datos del formulario
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Servir archivos estáticos (HTML, CSS, JS)
app.use(express.static('public'));

// Página de inicio
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/public/index.html');
});

// Ruta para procesar el pago
app.get('/comprar', (req, res) => {
    const { producto, precio } = req.query;

    res.send(`
        <html>
            <body>
                <h1>Compra: ${producto}</h1>
                <p>Precio: $${precio} USD</p>
                <form action="/procesar-pago" method="POST">
                    <input type="hidden" name="producto" value="${producto}">
                    <input type="hidden" name="precio" value="${precio}">
                    <button type="submit">Pagar con tarjeta</button>
                </form>
            </body>
        </html>
    `);
});

// Ruta para procesar el pago con Stripe
app.post('/procesar-pago', async (req, res) => {
    const { producto, precio } = req.body;

    try {
        const paymentIntent = await stripe.paymentIntents.create({
            amount: precio * 100, // El precio debe estar en centavos
            currency: 'usd',
            description: producto
        });

        res.send(`
            <html>
                <body>
                    <h2>Pago exitoso</h2>
                    <p>Has pagado $${precio} USD por ${producto}</p>
                </body>
            </html>
        `);
    } catch (error) {
        console.error(error);
        res.send('Error al procesar el pago.');
    }
});

app.listen(port, () => {
    console.log(`Servidor escuchando en http://localhost:${port}`);
});/mi-tienda
│
├── /public
│   ├── index.html
│   ├── style.css
│   └── script.js
│
├── /server
│   └── app.js
└── package.json<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Tienda Online</title>
    <link rel="stylesheet" href="/public/style.css">
</head>
<body>
    <header>
        <h1>Bienvenido a Mi Tienda</h1>
        <nav>
            <a href="#">Inicio</a>
            <a href="#productos">Productos</a>
            <a href="#">Contacto</a>
        </nav>
    </header>

    <section id="productos">
        <h2>Productos Destacados</h2>
        <div class="producto">
            <h3>Curso de Trading</h3>
            <p>$99 USD</p>
            <button onclick="irAPagar('Curso de Trading', 99)">Comprar</button>
        </div>
        <div class="producto">
            <h3>Libro de Trading</h3>
            <p>$20 USD</p>
            <button onclick="irAPagar('Libro de Trading', 20)">Comprar</button>
        </div>
    </section>

    <footer>
        <p>&copy; 2024 Mi Tienda Online</p>
    </footer>

    <script src="/public/script.js"></script>
</body>
</html>body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #333;
    color: white;
    padding: 10px;
    text-align: center;
}

nav a {
    color: white;
    margin: 10px;
    text-decoration: none;
}

#productos {
    padding: 20px;
    display: flex;
    justify-content: center;
    gap: 20px;
}

.producto {
    background-color: white;
    padding: 10px;
    border: 1px solid #ddd;
    text-align: center;
    width: 200px;
}

button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}function irAPagar(nombreProducto, precio) {
    window.location.href = `/comprar?producto=${nombreProducto}&precio=${precio}`;
}npm init -y
npm install express stripe body-parserconst express = require('express');
const bodyParser = require('body-parser');
const stripe = require('stripe')('sk_test_yourStripeSecretKey'); // Reemplaza con tu clave secreta de Stripe

const app = express();
const port = 3000;

// Middleware para procesar datos del formulario
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Servir archivos estáticos (HTML, CSS, JS)
app.use(express.static('public'));

// Página de inicio
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/public/index.html');
});

// Ruta para procesar el pago
app.get('/comprar', (req, res) => {
    const { producto, precio } = req.query;

    res.send(`
        <html>
            <body>
                <h1>Compra: ${producto}</h1>
                <p>Precio: $${precio} USD</p>
                <form action="/procesar-pago" method="POST">
                    <input type="hidden" name="producto" value="${producto}">
                    <input type="hidden" name="precio" value="${precio}">
                    <button type="submit">Pagar con tarjeta</button>
                </form>
            </body>
        </html>
    `);
});

// Ruta para procesar el pago con Stripe
app.post('/procesar-pago', async (req, res) => {
    const { producto, precio } = req.body;

    try {
        const paymentIntent = await stripe.paymentIntents.create({
            amount: precio * 100, // El precio debe estar en centavos
            currency: 'usd',
            description: producto
        });

        res.send(`
            <html>
                <body>
                    <h2>Pago exitoso</h2>
                    <p>Has pagado $${precio} USD por ${producto}</p>
                </body>
            </html>
        `);
    } catch (error) {
        console.error(error);
        res.send('Error al procesar el pago.');
    }
});

app.listen(port, () => {
    console.log(`Servidor escuchando en http://localhost:${port}`);
});
