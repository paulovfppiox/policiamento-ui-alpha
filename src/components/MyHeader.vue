<template>
    <header>
		<nav class="d-flex justify-content-between align-items-center py-3">
			<div class="menu-btn">
				<b-button variant="primary" v-if="showButtonAndAvatar" v-b-toggle.sidebar-backdrop class="ml-5"> Menu </b-button>
				<a href="/home" >
					<i v-if="showButtonAndAvatar" class="home-btn fas fa-home fa-lg"></i>
				</a>
			</div>
			
			<div class="mx-auto nome-app">
				Sistema de Gestão Operacional de Policiamento
			</div>

			<div class="header-left-comps d-flex justify-content-end align-items-center">
				<img class="detran-logo mr-2" src="https://raw.githubusercontent.com/paulovfppiox/policiamento-assets/main/logo-det-horiz.png">
				<b-avatar v-if="showButtonAndAvatar" variant="info" rounded></b-avatar>
				<div v-if="showButtonAndAvatar" class="user-data">
					{{ USER_FIRSTNAME }}
					{{ USER_LASTNAME  }} <br>
					{{ USER_MATRICULA }} <br>
					<i class="fas fa-sign-out-alt"></i>
					<a href="/">Sair</a>
				</div>
			</div>
		</nav>

		<b-sidebar id="sidebar-backdrop" title="Menu" bg-variant="dark" :backdrop-variant="variant" no-header-close backdrop shadow>
		<!-- <b-sidebar close-icon="x" id="sidebar-variant" title="Menu" bg-variant="dark" text-variant="light" shadow>-->
		<div class="px-3 py-2">
			
			<div id="menu-items">
				<ul>
					<li><a href="/add-operacoes"> 
						<i class="fas fa-plus"></i>  Cadastra Operações </a>
					</li>
					<li><a href="/historico-operacoes"> 
						<i class="fas fa-search"></i> Histórico de Operações </a>
					</li>
					<li><a href="/historico-equipamentos"> 
						<i class="fas fa-timeline"></i> Histórico de Equipamentos </a>
					</li>
					
					<li><a href="/relatorios-operacoes"> 
						<i class="fas fa-chart-bar"></i> Relatórios Gerais </a>
					</li>
				</ul>
			</div>
		</div>
		</b-sidebar>

	</header>
</template>

<script>
/* eslint-disable no-mixed-spaces-and-tabs */
export default 						{
  name:'MyHeader',
  data()			{	
		return {
			variant: 'dark',
			menuActive: false 
		}
  },
  computed: 	
  {
		/** Exibe o botão menu e avatar apenas nas telas que não sejam o Login. **/
		showButtonAndAvatar() 		{
			return !( this.$route.name === 'appLogin' ) 
		},
		USER_FIRSTNAME()	  		{
			//-------- Local --------
			const user = localStorage.getItem('user');
			// console.log(" --> User? = " + JSON.parse( user ).nome );
			console.log(" --> User? = " + JSON.parse( user ).nome );
			return ( JSON.parse( user ).nome ).toUpperCase();
			// return ( this.$store.state.user.nome ).toUpperCase();
		},
		USER_LASTNAME()		  		{
			const user = localStorage.getItem('user');
			console.log(" --> User? = " + JSON.parse( user ).sobrenome );
			return ( JSON.parse( user ).sobrenome ).toUpperCase();
		},
		/* USER_LASTNAME_OLD()		  		{
			console.log( this.$route.name + ' || Last Name ==>> ' + this.$store.state.user.nome );
			return ( this.$store.state.user.sobrenome ).toUpperCase();
		},*/
		USER_MATRICULA()			{
			return this.$store.state.user.matricula;
		},
  },
  methods:		{
		openMenu: function()		{
			this.menuActive = true;
		},
		closeMenu: function()		{
			this.menuActive = false;
		},
		showRouteName() 			{
			alert(this.$route.name);
		}
  }
}
</script>

<!-- Escopado só pro compo --> 
<style scoped>
	header 	  {
		background-color: var(--color-background-nav);
		width:100%;
		height: 60px;
		/* Isso está configurado dentro da classe global.container*/
		display: flex;
		justify-content: center; 
		align-items: center;
	}

	nav {
		display: flex; 
		justify-content: space-between;
		height: 60px;
		align-items: center;
	}

	#logo 	{
		width: 150px;
	}

	#menu-button	{
		width: 30px;
	}

	/* Fixa, pq será um retângulo 
	/* VH - Viewheigth - altura total da view  */
	#menu-overlay	{
		position: fixed; 
		top:0;
		left:0;
		width: 40%;
		height: 100vh; 
		background-color: #000000;
		opacity: 0.5;
	}
	#menu-logo	{
		width:110px;
		margin-top:30px;
		margin-bottom: 10px;
	}
	#menu-items {
		position: fixed;
		top:0;
		right:0;
		width:60%;
		height:100vh;
		display:none;
		flex-direction:column;
		justify-content: flex-start;
		align-items:center;
	}
	
	#menu-items.active	{
		display: flex;
	}

	ul {
		
		list-style: none;
		text-align: center;
	}

	ul li	{
		margin: 20px 0px;
		text-decoration: none;
		width: 250px;
		padding: 10px;
	}

	#menu-items ul li:hover {
 		 background-color: var(--color-background-nav);
		 padding: 10px; 
		 width: 250px;
		 margin: 20px 0px;
	}

	ul li a	{
		color: var(--color-text-light);
		text-decoration: none;
	}

	
	/* o media query, serve para definir estilos p/ diferentes dispositivos*/
	@media (min-width:620px) {
		
		/* essas classes não serão exibidos no Desktop */
		#menu-button,
		#menu-logo,
		#menu-overlay{
			display: none;
		}

		#menu-items{
			display: flex;
			position: static;
			height: 60px;
			width: auto;
			justify-content:stretch;
			align-items:center;
		} 
	}

	nav {
		top: 0;
		left: 0;
		width: 100%;
		height: 60px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		z-index: 999;
	}
	.detran-logo 	{
		width: 30%;
		height: 30%;
	}
	.menu-btn	{
		margin-left: 2%;
		z-index: 10;
	}
	.nome-app	{
		position: absolute;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
		color:#fff9f9;
		font-weight: bold;
		font-size: 18px;
		left: 0;
  		right: 0;	
		text-align: center;
	}

	/*Componentes à esquerda no header*/
	.header-left-comps	{
		margin-right: 2%;	
	}
	.user-data	{
		margin: 2%;
		font-size: 12px;
		color:rgb(255, 255, 255);
	}
	.home-btn	{
		color:rgb(255, 255, 255);
		padding-left: 20px;
	}
</style>

