<template>
  <main>
    <!-- <b-button @click="showModal()">Show Modal</b-button>-->
    <my-modal id="myModal" title="Confirmation" message="Are you sure?" :visible="modalIsVisible" @update:visible="modalIsVisible = $event" />
    <br>
    <h4 class="card-title text-center titulo-paginas"> Seja Bem-Vindo </h4>
    <!-- <DisplayOperacoes/> -->
    <!-- <AppDashboard></AppDashboard> -->
</main>
</template>
<script>
import MyModal from '@/components/MyModal.vue';
// import AppDashboard from '@/components/AppDashboard.vue';
import axios from 'axios';
export default      
{
  name: 'RelatoriosOperacoes',
  components: {
    MyModal,
    // AppDashboard
   // DisplayOperacoes
  },
  data()      {
      return {
          modalIsVisible: false,
          listaAgentes: [],
      }
  },
  created()  
  {   
      this.getListaAgentes();
      // console.log( "AppHome =>> " + JSON.stringify( this.listaAgentes ) );
  }, 
  computed: {
    LISTA_AGENTES()      {
        return this.$store.state.listaAgentes;
    }
  },
  methods: {
    showModal() {
      this.modalIsVisible = true;
    },
    getListaAgentes() 
    {
      var sendData =  {
          dados: {
              entidade: 'agente',
              operacao: 'consultar'
          }
      };
      // console.log( "SEND >>> " + JSON.stringify( sendData ));
      axios.post( this.$SERVICES_ENDPOINT_URL, sendData )
        .then( response => {
          
            // Assuming the response data is an array of objects with 'value' and 'text' properties
            // console.log( "DATA >>> " + JSON.stringify( response.data ) );

            if ( ( typeof response.data ) == 'object' )  {
                   var agentes = response.data;
                   /* console.log('-DATA == ' + JSON.stringify(  data )  );
                   /* console.log( "MSG = " + data.message + "|| code= " + data.code );*/
                   if ( agentes.code == 0 )  {
                        
                        this.listaAgentes = agentes.data;
                        // console.log( "Aqui 1 >> " + JSON.stringify( this.listaAgentes) );
                        /*for( var i=0; i<this.listaAgentes.length; i++)  {
                             this.agentesLabelBD[i] = this.listaAgentes[ i ].matricula + " - " + this.listaAgentes[ i ].nome;               
                             console.log(  this.agentesLabelBD[i] );
                        }*/
                   } 
            }
            this.$store.commit('setListaAgentes', this.listaAgentes );

            /****
             this.selectOptions = response.data.map(item => ({
              value: item.value,
              text: item.text,
            }));
            ****/
        })
        .catch(error => {
          console.error(error);
        });
    },
  }
}
</script>


<style>
.heading{
  text-align: center;
  margin-top: 50px;
}
.wrapper{
  display: flex;
  align-items: center;
  justify-content: center;
}
.pie-wrap{
  border: 2px solid lightgrey;
  width: 300px;
  height: 300px;
  margin: 10% 50px;
  position: relative;
  border-radius: 50%;
  overflow: hidden;
  color: black;
}
.pie-wrap .entry{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* the individual entries */
.sky-blue{
  background-color: lightskyblue;
  height:50%;
  width: 50%;
  display: block;
}
.light-yellow{
  background-color: lightyellow;
  height:50%;
  width: 50%;
}
.pink{
  background-color: rgb(52, 115, 139);
  height:50%;
  position: absolute;
  top: 0px;
  right: 0;
  width: 50%;
  clip-path: polygon(0 0, 100% 0%, 0% 100%);
}
.purple{
  background-color: purple;
  height:50%;
  width: 50%;
  right: 0;
  top: 0;
  position: absolute;
  clip-path:polygon(0% 100%, 100% 0%, 100% 100%);
}
.green{
  background-color: limegreen;
  height:50%;
  width: 50%;
  right: 0;
  top: 50%;
  position: absolute;
  clip-path:polygon(0% 0%, 100% 0%, 100% 100%);
}
.wheat{
  background-color: wheat;
  height:50%;
  width: 50%;
  right: 0;
  top: 50%;
  position: absolute;
  clip-path:polygon(0% 0%, 100% 100%, 0% 100%);
}

/* the entry text styles */
.pie-wrap .purple p{
  position: absolute;
  top: 140px;
  color: white;
 }
 .pie-wrap .purple p:first-child{
   top: 120px;
 }
 .pie-wrap .green p{
   position: absolute;
   top: 20px;
  }
  .pie-wrap .green p:first-child{
   top: 0px;
 }
  .pie-wrap .pink p, .pie-wrap .wheat p{
   position: absolute;
   left: 20px;
   top: 80px;
  }
 .pie-wrap .pink, .pie-wrap .wheat{
   justify-content: flex-start;
 }
 .pie-wrap .pink p:first-child, .pie-wrap .wheat p:first-child{
   top: 100px;
 }

 /* hide food name and reveal when hovered on */
 .entry .entry-value{
  display: none;
  transition: all 500ms linear;
}
.entry:hover .entry-value{
  display: block;
}
.entry{
  transition: all 500ms linear ;
}
.entry:hover{
  filter: invert();
}

/* label style */
.key-wrap label{
  display: block;
  border: 1px solid;
  width: 100px;
  text-align: center;
  padding: 10px 15px;
  cursor: pointer;
  margin-bottom: 8px;
}
input[type="radio"]{
  display: none;
}

/* background colors for the entry keys */
.rice-label{
  background-color: lightyellow;
}
.beans-label{
  background-color:  rgb(52, 115, 139);
}
.plantain-label{
  background-color: purple;
  color: white;
}
.potato-label{
  background-color: limegreen;
}
.yam-label{
  background-color: wheat;
}
.pasta-label{
  background-color: skyblue;
}

/* the texts displayed below */
.text{
  display: none;
  margin-left: -50px;
  position: absolute;
  margin-top: 50px;
  font-size: 20px;
  padding-bottom: 4px;
  padding-top: 4px;
  border-bottom: 5px solid ;
  border-top: 5px solid ;
}
.key-wrap label:active{
  opacity: .5;
}
.rice-key:checked ~ .rice-text, .beans-key:checked ~ .beans-text, .plantain-key:checked ~ .plantain-text,  .potato-key:checked ~ .potato-text, .yam-key:checked ~ .yam-text, .pasta-key:checked ~ .pasta-text{
 display: block;
}

.rice-text{
  border-color: black;
}
.beans-text{
  border-color:   rgb(52, 115, 139);
}
.plantain-text{
  border-color:  purple;
}
.potato-text{
  border-color:  limegreen;
}
.yam-text{
  border-color:  wheat;
}
.pasta-text{
  border-color:  skyblue;
}


/***---------------- */

</style>