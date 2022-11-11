<template>
  <div class="container">
      <label class="text-center"> Formulario de Prueba</label>
     <div class="col m12 card-panel">
        <form @submit.prevent="agregarUsuario">
              <div class="row">
                  <div class="col m4">
                      <label>Nombre</label>
                      <input type="text" v-model="nombre">
                  </div>
                  <div class="col m4">
                      <label>Apellido</label>
                      <input type="text" v-model="apellido">
                  </div>
              </div>     
  
                  <div class="row">
                          <div class="col m4">
                              <label>Edad</label>
                              <input type="number" v-model="edad">
                          </div>
                          <div class="col m4">
                              <label>Estado Civil</label>
                                  <select v-model="estadoCivil">
                                      <option value="">Seleccione</option>
                                      <option v-for="estado in estados_civiles" :value="estado.id" :key="estado">{{estado.descripcion}}</option>
                                  </select>
                          </div>
                          <div class="col m4">
                              <label>Correo</label>
                                  <input type="email" v-model="correo">
                          </div>
                  </div>
                  <div class="row">
                      <form @submit.prevent="agregarCargo">
                          <div class="col-sm-4 card-panel">
                              <label>Cargos</label>
                              <input type="text" v-model="rol">
                              <button type="submit" class="btn indigno darken-3">AGREGAR CARGO<i class="material-icons right">send</i></button>
                              <br>
                              <ul>
                                  <li class="list-item" v-for="cargo in cargos " :key="cargo">
                                   {{cargo.id}} - {{cargo.descripcion}}
                                  </li>
                              </ul>
                          </div>
                      </form>
                  </div>
                  <div class="row">
                      <div class="col m4">
                      <label><input type="checkbox" @click="PruebaCheck()"  v-model="suscribirse"><span>Suscribirse al boletin de noticias</span></label>
                      </div>
                  </div>
                  <div class="row">
                      <button type="submit" class="btn indigo darken-4">AGREGAR USUARIO <i class="material-icons">add_circle</i></button>
                  </div>
        </form>
     </div>  
      <div class="row">
          <div class="col-m1">
              <button class="btn btn-danger" @click="mostrarTable">Usuarios registrados</button>
          </div>
      </div>
     <div class="row">
         <div class="col m12">
             <table class="table bordered striped" v-show="tabledisable">
                 <thead>
                     <tr>
                         <th>Nombre</th>
                          <th>Apellido</th>
                           <th>Edad</th>
                            <th>EstadoCivil</th>
                             <th>Correo</th>
                              <th>Cargos</th>
                               <th>Suscrito</th>
                                <th>Editar</th>
                                 <th>Eliminar</th>
                     </tr>
                 </thead>
                 <tbody>
                      <tr v-for="(usuario, index) in usuarios" :key="usuario">
                          <td>{{usuario.nombre}}</td>
                              <td>{{usuario.apellido}}</td>
                              <td>{{usuario.edad}}</td>
                              <td>{{usuario.estado_civil}}</td>
                              <td>{{usuario.correo}}</td>
                              <td >
                                  <ul>
                                      <li v-for="cargo in usuario.cargos" :key="cargo">{{cargo.id}} - {{cargo.descripcion}}</li>
                                  </ul>
                              </td>
                              <td><input type="checkbox"  v-model="usuario.suscribirse"><span></span></td>
                              <td><a href="#" @click="editarDatos(index)"><i class="material-icons">create</i></a></td>
                              <td ><a href="#" @click="quitarDatos(index)"><i class="material-icons">delete</i></a></td>
                      </tr>
                 </tbody>
             </table>
         </div>
      </div> 
  </div>
  
  </template>
  
  <script>
  import M from 'materialize-css'
      export default {
          data(){
              return {
                  indice: -1,
                  nombre: '',
                  apellido: '',
                  edad: '',
                  estadoCivil: '',
                  correo: '',
                  suscribirse: false,
                  rol: '',
                  cargos: [],
                  usuarios: [], 
                  tabledisable: false,         
  
                  select_instancia: [] ,     
                  estados_civiles: [
                      {
                          id: 'S',
                          descripcion: 'Soltero'
                      },
                      {
                          id: 'C',
                          descripcion: 'Casado',
                      },
                      {
                          id: 'V',
                          descripcion: 'Viudo'
                      },
                      {
                          id: 'D',
                          descripcion: 'Divorciado'
                      }
                  ]
              }
          },
          methods: {
              agregarUsuario()
              {
                  var data = {
                      nombre: this.nombre,
                      apellido: this.apellido,
                      edad: this.edad,
                      estado_civil: this.estadoCivil,
                      correo: this.correo,
                      suscribirse: this.suscribirse,
                      cargos: this.cargos
                  };
                  if(this.nombre != "" && this.apellido != ""  && this.edad != "" && this.estadoCivil != "" && this.correo != "")
                  {
                      if(this.indice == -1){                   
                          this.usuarios.push(data);
                      }
                      else{
                          this.usuarios[this.indice] = data;
                      }
                      console.log(this.index, this.usuarios)
                  }
                  else 
                  {
                      alert("Ingrese todos los datos por favor..");
                  }
  
                  //despues de agregar a la tablar vacio el formulario
                  this.nombre= '',
                  this.apellido= '', '',
                  this.edad= '', '',
                  this.estadoCivil= '', '',
                  this.correo= '', '',
                  this.suscribirse= '',
                  this.rol= '', 
                  this.cargos= []
              },
              agregarCargo(){
                  var total = this.cargos.length;
  
                      console.log(total);
                  var ultimo = 0;
            
                  if(total > 0)
                  {
                      ultimo = this.cargos[total - 1].id;
                  }
                  var data = 
                  {
                      id: ultimo + 1,
                      descripcion: this.rol
                  };
                  if(this.rol != "")
                  {
                      this.cargos.push(data);
                  this.rol= ''
                  console.log("pasatiempos",this.cargos)
                  }
                  else
                  {
                      alert("Ingrese el cargo por favor");
                  }   
              },
              quitarDatos(index){
                  if(!confirm('Desea eliminar este registro')) return;
                  this.usuarios.splice(index, 1);
              },
              mostrarTable(){
                  this.tabledisable = true;
              },
              editarDatos(index)
              {
                  var usuario = this.usuarios[index];
                  this.indice = index;
                  this.nombre = usuario.nombre;
                  this.apellido = usuario.apellido;
                  this.edad = usuario.edad;
                  this.correo = usuario.correo;
                  this.estadoCivil = usuario.estado_civil;
                  this.cargos = usuario.cargos
              },
              PruebaCheck()
              {        
                  var prueba = false;
                  prueba  = !this.suscribirse
                  console.log(prueba);            
              }
          },
          mounted()
          {
              //console.log(this.suscribirse);
              var elems = document.querySelectorAll('select');
              this.select_instancia = M.FormSelect.init(elems, null);
          }
  }
  </script>
  
  <style lang="scss" scoped>
  
  </style>