<template>
    <div>

        <div id="app" class="container">
    <div class="row">
      <div class="col-lg-5 col-md-8 col-sm-7 col-xs-6">
        <div class="form-group">
          <div class="titulo"> <h1>{{titulo}}</h1> </div>
          <div v-if="mostrarMensaje" class="mensaje"> {{mensaje}}
            <button class="cierre" @click="mostrarMensaje=false">x</button>
          </div>
        </div>


      <button class="btn btn-info" @click="altaarticulo=true">Ingresar articulo</button>
      <button class="btn btn-info" @click="buscararticulo=true">Buscar articulos</button>
          <div class="control" v-if="buscararticulo">
           <input type="text"  placeholder="Buscar articulos..." v-model="busqueda" >
           <div class="btn-group">
            <button class="btn btn-outline-danger" @click="buscararticulo=false,mostrarArticulos()">Cerrar</button>
           </div>
          </div>


        <table class="table table-bordered">
          <thead>
            <tr class="columna_nombre">
              <th>
                Id
              </th>
              <th>
                Nombre
              </th>
              <th>
                Rubro
              </th>
              <th>
                Acciones
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="columna_nombre" v-for="(articulo,index) in buscarArticulos">
              <td>
                {{articulo.id}}
              </td>
              <td>
                {{articulo.nombre}}
              </td>
              <td>
                {{articulo.rubro_nombre}}
              </td>
              <td>
                <button class="btn btn-primary" @click="editararticulo=true,cargaArticulo(articulo)">
                  Editar
                </button>
                <button class="btn btn-danger" @click="bajaarticulo=true,cargaArticulo(articulo)">
                  Eliminar
                </button>
              </td>
            </tr>


          </tbody>
        </table>

      </div>

      <div class="col-lg-7  col-md-4 col-sm-5 col-xs-6 data">
        {{ $data }}
      </div>

    </div>


    <div class="row">
      <div class="contenedor" v-if="altaarticulo">
        <div class="VentanaModal" >
          <div class="cabecera tituloventana">
            <button class="cierre btn btn-primary" @click="altaarticulo=false"><font color="#35586F">X</font></button>
            <h1>Ingresando Articulo perro</h1>
          </div>
          <div class="contenido">
              <div class="form-group campo">
                Nombre del articulo <br>
                <input type="text" autofocus name="nombre" id="nombre" class="form-control" v-model="articuloRegistro.nombre">
              </div>
              <div class="form-group campo">
                <label for="" >Rubro</label>
                <select name="id_rubro" id="id_rubro"  class="form-control" v-model="articuloRegistro.id_rubro">
                  <option value="">Seleccionar rubro</option>
                  <option v-for="item in rubros" :value="item.id">{{item.nombre}}</option>
                </select>
              </div>
              <button class="btn  btn-outline-primary" @click="altaarticulo=false;ingresarArticulos()">Ingresar Articulo</button>
              <button class="btn btn-outline-danger" @click="altaarticulo=false,close">Cancelar</button>
          </div>
        </div>
      </div>

      <div class="contenedor" v-if="editararticulo">
       <div class="VentanaModal" >
         <div class="cabecera tituloventana">
           <button class="cierre btn btn-info" @click="editararticulo=false"><font color="#1280F5">X</font></button>
           <h1 >Modificar Articulo</h1>
         </div>
         <div class="contenido">
             <div class="form-group campo">
               Nombre del articulo <br>
               <input type="text" name="nombre" id="nombre" class="form-control" v-model="articuloRegistro.nombre">
             </div>
              <div class="form-group campo">
                <label for="" >Rubro</label>
                <select name="id_rubro" id="id_rubro"  class="form-control" v-model="articuloRegistro.id_rubro">
                  <option value="">Seleccionar rubro</option>
                  <option v-for="item in rubros" :value="item.id">{{item.nombre}}</option>
                </select>
              </div>


             <button class="btn  btn-outline-info" @click="editararticulo=false;modificarArticulos()">Editar</button>
             <button class="btn btn-outline-danger" @click="editararticulo=false,mostrarArticulos()">Cancelar</button>
         </div>
       </div>
     </div>

      <div class="contenedor" v-if="bajaarticulo">
        <div class="VentanaModal" >
          <div class="cabecera tituloventana">
            <button class="cierre btn btn-danger" @click="bajaarticulo=false"><font color="EF522C">X</font></button>
            <h1>Eliminar Articulo</h1>
          </div>
          <div class="contenido">
              <div class="form-group campo">
                Nombre del articulo <br>
                <input type="text" readonly="readonly" name="nombre" id="nombre" class="form-control" v-model="articuloRegistro.nombre">
              </div>
              <button class="btn  btn-outline-danger" @click="bajaarticulo=false;borrarArticulos()">Eliminar Articulo</button>
              <button class="btn  btn-outline-danger" @click="bajaarticulo=false;close">Cancelar</button>
          </div>
        </div>
    </div>


    </div>
  </div>



    </div>
</template>


<script>
export default {
    name:"Listado",

    data: function(){

            return{

                titulo:"Stock",
                articulos:[],
                rubros:[],

                articuloRegistro:[],
                mensaje:'',
                busqueda:'',
                indice:0,

                altaarticulo:false,
                bajaarticulo:false,
                editararticulo:false,
                mostrarMensaje:false,
                buscararticulo:false,
                altarubro:false,
            }
        },

    mounted:function(){
    this.mostrarArticulos();
    this.mostrarRubros();
    },   
  
    
    methods:{
      mostrarArticulos:function(){

        axios.post("http://localhost/sistemas2020/sistemas2020_php/php/listar.php")
        .then(function(response){
          if (response.data.articulos===undefined) {
            app.articulos=[];
          } else {
            console.log(response.data.articulos);
            app.articulos=response.data.articulos;
          }
        })
      },

      mostrarRubros:function(){
          axios.get("http://localhost/sistemas2020/sistemas2020_php/php/rubros.php")
          .then(function(response){
            if (response.data.rubros===undefined) {
              app.rubros=[];
            } else {
              console.log("LISTADO RUBROS");
              console.log(response.data.rubros);
              app.rubros=response.data.rubros;
            }
          })
        },


      ingresarArticulos:function(){
   
       let formdata=new FormData();
       formdata.append("nombre",this.articuloRegistro.nombre);
       formdata.append("id_rubro",this.articuloRegistro.id_rubro);
       axios.post("http://localhost/sistemas2020/sistemas2020_php/php/ingresar.php",formdata)
       .then(function(response){
         app.mostrarMensaje=false;
         app.mensaje=response.data.mensaje;
           /*
         app.articulos.push({  id:response.data.ultimoId,
                               nombre:app.articuloRegistro.nombre});
          */
   
         app.mostrarArticulos();
        })
      },






    cargaArticulo:function(dato){
      console.log(dato);
      this.articuloRegistro=dato;
    },


    modificarArticulos:function(){
      let formdata=new FormData();
      formdata.append("id",this.articuloRegistro.id);
      formdata.append("nombre",this.articuloRegistro.nombre);
      formdata.append("id_rubro",this.articuloRegistro.id_rubro);
      console.log(this.articuloRegistro);
      axios.post("http://localhost/sistemas2020/sistemas2020_php/php/editar.php",formdata)
      .then(function(response){
        app.mostrarMensaje=false;
        app.mensaje=response.data.mensaje;
        /*
        app.articulos.push({  id:response.data.ultimoId,
                              nombre:app.articuloRegistro.nombre});
         */
        app.mostrarArticulos();
      })
    },

    borrarArticulos:function(){

      let formdata=new FormData();
      formdata.append("id",this.articuloRegistro.id);

      axios.post("http://localhost/sistemas2020/sistemas2020_php/php/borrar.php",formdata)
      .then(function(response){
        app.mostrarMensaje=false;
        app.mensaje=response.data.mensaje;
         /*
        app.articulos.push({  id:response.data.ultimoId,
                              nombre:app.articuloRegistro.nombre});
        */
        app.mostrarArticulos();
      })
    },


  },
   computed:{
        buscarArticulos:function(){
                return this.articulos.filter((articulo)=>articulo.nombre.includes(this.busqueda))
        },
    }


    
}
</script>