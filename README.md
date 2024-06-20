# Ionic-Construindo-uma-Pokédex-usando-a-API-do-Pokémon

Para construir uma Pokédex utilizando Ionic, podemos seguir alguns passos básicos e utilizar os recursos do framework para criar uma aplicação funcional e visualmente atraente. Abaixo, vou dar exemplos de como você pode estruturar e implementar alguns módulos essenciais para esse projeto.

### Estrutura do Projeto Ionic

1. **Inicialização do Projeto**
   
   Primeiro, certifique-se de ter o Ionic instalado globalmente:
   ```bash
   npm install -g @ionic/cli
   ```

   Crie um novo projeto Ionic:
   ```bash
   ionic start pokedex blank --type=angular
   cd pokedex
   ```

2. **Instalação de Dependências**
   
   Você pode usar a PokéAPI (API oficial de Pokémon) para obter os dados dos Pokémon. Instale o pacote `axios` para fazer requisições HTTP:
   ```bash
   npm install axios
   ```

3. **Componentes e Módulos**

   **Pokémon Service**
   Crie um serviço para gerenciar as requisições à API e armazenar os dados dos Pokémon.
   
   ```typescript
   // src/app/services/pokemon.service.ts
   import { Injectable } from '@angular/core';
   import axios from 'axios';

   @Injectable({
     providedIn: 'root'
   })
   export class PokemonService {

     private baseUrl = 'https://pokeapi.co/api/v2/';

     constructor() { }

     async getPokemonList(limit: number = 20, offset: number = 0) {
       const url = `${this.baseUrl}pokemon?limit=${limit}&offset=${offset}`;
       const response = await axios.get(url);
       return response.data;
     }

     async getPokemonDetails(pokemonName: string) {
       const url = `${this.baseUrl}pokemon/${pokemonName}`;
       const response = await axios.get(url);
       return response.data;
     }

   }
   ```

   **Página de Listagem de Pokémon**
   Crie uma página para exibir uma lista de Pokémon com seus nomes e imagens.

   ```html
   <!-- src/app/pokemon-list/pokemon-list.page.html -->
   <ion-header>
     <ion-toolbar>
       <ion-title>
         Pokémon List
       </ion-title>
     </ion-toolbar>
   </ion-header>

   <ion-content>
     <ion-list>
       <ion-item *ngFor="let pokemon of pokemonList">
         <ion-avatar slot="start">
           <img [src]="pokemon.spriteUrl">
         </ion-avatar>
         <ion-label>{{ pokemon.name }}</ion-label>
       </ion-item>
     </ion-list>
   </ion-content>
   ```

   ```typescript
   // src/app/pokemon-list/pokemon-list.page.ts
   import { Component, OnInit } from '@angular/core';
   import { PokemonService } from '../services/pokemon.service';

   @Component({
     selector: 'app-pokemon-list',
     templateUrl: './pokemon-list.page.html',
     styleUrls: ['./pokemon-list.page.scss'],
   })
   export class PokemonListPage implements OnInit {

     pokemonList: any[] = [];

     constructor(private pokemonService: PokemonService) { }

     ngOnInit() {
       this.loadPokemonList();
     }

     async loadPokemonList() {
       const data = await this.pokemonService.getPokemonList();
       this.pokemonList = data.results.map((result: any) => ({
         name: result.name,
         spriteUrl: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${result.url.split('/')[6]}.png`
       }));
     }

   }
   ```

4. **Estilização**

   Use o sistema de estilização do Ionic (CSS, SASS, etc.) para personalizar o visual da aplicação conforme necessário.

5. **Navegação**

   Configure a navegação entre páginas usando o sistema de rotas do Angular (ou Ionic Navigation).

6. **Execução**

   Execute a aplicação para ver como está ficando:
   ```bash
   ionic serve
   ```

### Considerações Finais

Este é um esqueleto básico para construir uma Pokédex utilizando Ionic. Poderemos expandir este projeto adicionando mais funcionalidades, como detalhes de Pokémon individuais, pesquisa, filtros, etc. O Ionic oferece uma série de componentes visuais e recursos que podem ser explorados para melhorar a experiência do usuário na aplicação. Certifique-se de consultar a documentação do Ionic para mais detalhes sobre cada componente e recurso utilizado.
