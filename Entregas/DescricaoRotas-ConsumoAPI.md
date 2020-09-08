# Rotas

 ## app.routing.module.ts

O blog Seiva possuirá as rotas:   
**home-inicio** sendo essa a página de redirecionamento ao ingressar no site;   
**feed** onde o usuário será redirecionando após o login efetuado;   
**equipe** acesso aos integrantes do projeto bem como o acesso as suas redes sociais LinkedIn e GitHub;   
**sobre** com as informações sobre a rede social Seiva;   
**editar e deletar postagem** os usuários vizualizarão todas as suas postagens e poderão editar e/ou deletar as mesmas;   
**admin** acesso restrito apenas para o login admin;   
**cadastro tema** apenas o login admin possuirá acesso para cadastrar um novo tema;   
**editar e deletar tema** apenas o login admin possuirá acesso para editar e/ou deletar um tema.

### Routes

```json
    const routes: Routes = [
        { path: '', redirectTo: 'home-inicio', pathMatch: 'full' },
	    { path: 'home-inicio', component: HomeInicioComponent },
        { pathFeedComponent },: 'feed', component: 
        { path: 'equipe', component: EquipeComponent },
        { path: 'sobre', component: SobreComponent },
        { path: 'deletar-postagem/:id', component: DeletePostagemComponent },
        { path: 'editar-postagem/:id', component: PutPostagemComponent },
        { path: 'admin', component: AdminComponent },
        { path: 'cadastro-tema', component: PostTemaComponent },
        { path: 'editar-tema/:id', component: PutTemaComponent },
        { path: 'deletar-tema/:id', component: DeleteTemaComponent },
    ];
```
   
# Consumo API
## Model
As model's criadas precisam possuir os atributos iguais aos do declarados pelo back-end.
#### Postagem.ts
```json
    export class Postagem {
        public id: number;
        public titulo: string;
        public descricao: string;
        public data: Date;
        public tema: Tema
    }
```
#### Tema.ts
```json
    export class Tema {
        public id: number;
        public descricao: string;
        public postagem: Postagem []
    }
```
#### User.ts
```json
    export class User {
        public id: number;
        public nome: string;
        public usuario: string;
        public senha: string
    }
```
#### UserLogin.ts
```json
    export class UserLogin {
        public usuario: string;
        public senha: string;
        public token: string
}
```

## Service  
Criado os serviços:  
**auth** autenticação para logar e cadastrar-se, possuindo os eventos de 'btnSair()' e 'admin()';  
**postagem** com os end-point's GET, GETByID, POST, PUT e DELETE, possuindo os eventos de 'btnSair()' e 'btnLogin()';  
**tema** com os end-point's GET, GETByID, POST, PUT e DELETE.

### auth.service.ts 
```json
export class AuthService {
  constructor(private http: HttpClient) { }

    logar(userLogin: UserLogin) {
        return this.http.post('http://localhost:9000/usuario/logar', userLogin)
    }
    cadastrar (user: User) {
        return this.http.post('http://localhost:9000/usuario/cadastrar', user)
    }

    btnSair(){
        let ok = false
        let token = localStorage.getItem('token')
        if (token != null) {
        ok = true
        }
        return ok
    }

    admin(){
        let ok = false
        let email = localStorage.getItem('email')
        if(email == 'admin@admin.com'){
        ok = true
        }
        return ok
    }
}
```

### postagem.service.ts
```json
export class PostagemService {
  constructor(private http: HttpClient) { }

    logar(userLogin: UserLogin){
        return this.http.post('http://localhost:9000/usuario/logar', userLogin)
    }

    cadastrar(user: User){
        return this.http.post('http://localhost:9000/usuario/cadastrar', user)
    }

    token = {
        headers: new HttpHeaders().set('Authorization', localStorage.getItem('token'))
    }

    getAllPostagens() {
        return this.http.get('http://localhost:9000/postagem', this.token)
    }

    getByIdPostagem(id: number) {
        return this.http.get(`http://localhost:9000/postagem/${id}`, this.token)
    }

    postPostagem(postagem: Postagem) {
        return this.http.post('http://localhost:9000/postagem', postagem, this.token)
    }

    putPostagem(postagem: Postagem) {
        return this.http.put('http://localhost:9000/postagem', postagem, this.token)
    }

    deletePostagem(id: number){
        return this.http.delete(`http://localhost:9000/postagem/${id}`, this.token)
    }

    btnSair(){
        let ok = false
        let token = localStorage.getItem('token')
        if (token != null){
        ok = true
        }
        return ok
    }

    btnLogin(){
        let ok = false
        let token = localStorage.getItem('token')
        if (token == null){
        ok = true
        }
        return ok
    }
}
```

### tema.service.ts 
```json
export class TemaService {

  constructor(private http: HttpClient) { }

    token = {
        headers: new HttpHeaders().set('Authorization', localStorage.getItem('token'))
    }

    getAllTemas() {
        return this.http.get('http://localhost:9000/tema', this.token)
    }

    getByIdTema(id: number) {
        return this.http.get(`http://localhost:9000/tema/${id}`, this.token)
    }

    postTema(tema: Tema) {
        return this.http.post('http://localhost:9000/tema', tema, this.token)
    }

    putTema(tema: Tema) {
        return this.http.put('http://localhost:9000/tema', tema, this.token)
    }

    deleteTema(id: number){
        return this.http.delete(`http://localhost:9000/tema/${id}`, this.token)
    }
}
```


