<html>
<head>
    <h1><strong><big><center>Palacio de las Medicinas</center></big></strong></h1>
  
    
    <body background='background.png'></body>
    <meta charset="UTF-8">
    <title></title>
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" crossorigin="anonymous">
    <script>
        window.onload = function () {
            
           let baseDeDatos = [
                {
                    id: 1,
                    nombre: 'Panadol',
                    precio: 5.40,
                    imagen: 'https://soukare.com/image/catalog/Images/Pharmacy/Panadol-Extra-Web2.jpg'
                },
                {
                    id: 2,
                    nombre: 'Dorival',
                    precio: 3.50,
                    imagen: 'https://www.coide.com.gt/images/dorivalg.jpg'
                },
                {
                    id: 3,
                    nombre: 'Pei Pa Koa',
                    precio: 7.80,
                    imagen: 'https://www.asianfoodlovers.com/media/catalog/product/cache/8/image/750x750/9df78eab33525d08d6e5fb8d27136e95/j/i/jiom_pei_pa_kao_herbs_syrup.jpg'
                },
                {
                    id: 4,
                    nombre: 'Gastrigel',
                    precio: 4.80,
                    imagen:'https://www.laboratoriosrigar.com/wp-content/uploads/2018/03/gastrigel-cereza-sin-fondo.jpg'
                }

            ]
            let $items = document.querySelector('#items');
            let carrito = [];
            let total = 0;
            let $carrito = document.querySelector('#carrito');
            let $total = document.querySelector('#total');
            let $botonVaciar = document.querySelector('#boton-vaciar');

         
            function renderItems() {
                for (let info of baseDeDatos) {
                    
                    let miNodo = document.createElement('div');
                    miNodo.classList.add('card', 'col-sm-4');
                   
                    let miNodoCardBody = document.createElement('div');
                    miNodoCardBody.classList.add('card-body');
                  
                    let miNodoTitle = document.createElement('h5');
                    miNodoTitle.classList.add('card-title');
                    miNodoTitle.textContent = info['nombre'];
                  
                    let miNodoImagen = document.createElement('img');
                    miNodoImagen.classList.add('img-fluid');
                    miNodoImagen.setAttribute('src', info['imagen']);
                  
                    let miNodoPrecio = document.createElement('p');
                    miNodoPrecio.classList.add('card-text');
                    miNodoPrecio.textContent = info['precio'] + '$';
                    
                    let miNodoBoton = document.createElement('button');
                    miNodoBoton.classList.add('btn', 'btn-primary');
                    miNodoBoton.textContent = '+';
                    miNodoBoton.setAttribute('marcador', info['id']);
                    miNodoBoton.addEventListener('click', anyadirCarrito);
                  
                    miNodoCardBody.appendChild(miNodoImagen);
                    miNodoCardBody.appendChild(miNodoTitle);
                    miNodoCardBody.appendChild(miNodoPrecio);
                    miNodoCardBody.appendChild(miNodoBoton);
                    miNodo.appendChild(miNodoCardBody);
                    $items.appendChild(miNodo);
                }
            }

            function anyadirCarrito () {
                
                carrito.push(this.getAttribute('marcador'))
            
                calcularTotal();
             
                renderizarCarrito();
            }

            function renderizarCarrito() {
               
                $carrito.textContent = '';
    
                let carritoSinDuplicados = [...new Set(carrito)];
              
                carritoSinDuplicados.forEach(function (item, indice) {
                
                    let miItem = baseDeDatos.filter(function(itemBaseDatos) {
                        return itemBaseDatos['id'] == item;
                    });
                  
                    let numeroUnidadesItem = carrito.reduce(function (total, itemId) {
                        return itemId === item ? total += 1 : total;
                    }, 0);
                   
                    let miNodo = document.createElement('li');
                    miNodo.classList.add('list-group-item', 'text-right', 'mx-2');
                    miNodo.textContent = `${numeroUnidadesItem} x ${miItem[0]['nombre']} - ${miItem[0]['precio']}$`;
                   
                    let miBoton = document.createElement('button');
                    miBoton.classList.add('btn', 'btn-danger', 'mx-5');
                    miBoton.textContent = 'X';
                    miBoton.style.marginLeft = '1rem';
                    miBoton.setAttribute('item', item);
                    miBoton.addEventListener('click', borrarItemCarrito);
                   
                    miNodo.appendChild(miBoton);
                    $carrito.appendChild(miNodo);
                })
            }

            function borrarItemCarrito() {
                console.log()
               
                let id = this.getAttribute('item');
               
                carrito = carrito.filter(function (carritoId) {
                    return carritoId !== id;
                });

                renderizarCarrito();
                
                calcularTotal();
            }

            function calcularTotal() {
                
                total = 0;
                
                for (let item of carrito) {
                    
                    let miItem = baseDeDatos.filter(function(itemBaseDatos) {
                        return itemBaseDatos['id'] == item;
                    });
                    total = total + miItem[0]['precio'];
                }
                
                let totalDosDecimales = total.toFixed(2);
                
                $total.textContent = totalDosDecimales;
            }

            function vaciarCarrito() {
                
                carrito = [];
               
                renderizarCarrito();
                calcularTotal();
            }

           
            $botonVaciar.addEventListener('click', vaciarCarrito);

            
            renderItems();
        } 
    </script>
</head>
<body>
    <div class="container">
        <div class="row">
            <!-- Elementos generados a partir del JSON -->
            <main id="items" class="col-sm-8 row"></main>
            <!-- Carrito -->
            <aside class="col-sm-4">
                <h2>Carrito</h2>
                <!-- Elementos del carrito -->
                <ul id="carrito" class="list-group"></ul>
                <hr>
                <!-- Precio total -->
                <p class="text-right">Total: <span id="total"></span>&#36;</p>
                <button id="boton-vaciar" class="btn btn-danger">Vaciar</button>
   <center><form action="formulario de compra.html">
        <input type="submit" value="Comprar" />
       </form></center>
   
            </aside>
        </div>
    </div>

</body>
</html>
