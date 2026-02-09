# 📝 TodoList App — Android Moderno

> Uma aplicação de gerenciamento de tarefas robusta e escalável, desenvolvida para demonstrar o uso de Jetpack Compose, Arquitetura Reativa e o novo sistema de Navegação com Segurança de Tipos do Android.

## 🚀 Sobre o Projeto

Este projeto é uma demonstração de **Modern Android Development (MAD)**. O objetivo principal é exibir uma arquitetura limpa e escalável utilizando as ferramentas mais recentes do ecossistema Google, com foco especial na migração para a **Type-Safe Navigation** e integração robusta entre **Room** (offline) e **Firebase** (online).

## ✨ Funcionalidades

* **🔐 Autenticação Completa:** Fluxo de Login e Cadastro integrado ao Firebase Auth.
* **🧠 Navegação Inteligente:** Gerenciamento de sessão que impede o retorno indevido à tela de login após a autenticação.
* **tasks Gerenciamento de Tarefas (CRUD):**
    * Listagem reativa (StateFlow).
    * Criação de novas notas.
    * Edição de tarefas existentes com reutilização de UI.
* **💾 Persistência Offline:** Suporte total offline-first utilizando Room Database.
* **🎨 UI Moderna:** Interface construída 100% em **Jetpack Compose** com Material Design 3.

---

## 🛠 Stack Tecnológica

O projeto utiliza as bibliotecas mais recentes recomendadas (BOM 2025+):

| Categoria | Tecnologia | Versão/Detalhe |
| :--- | :--- | :--- |
| **UI** | Jetpack Compose | BOM 2025.12.00 |
| **Navegação** | Navigation Compose | 2.8.0 (Type-Safety) |
| **Injeção de Dep.** | Hilt | 2.51.1 |
| **Backend** | Firebase Auth | Com Play Services Coroutines |
| **Database** | Room | 2.6.1 |
| **Arquitetura** | MVVM | StateFlow & Lifecycle-aware |
| **Dados** | Kotlinx Serialization | Tráfego de objetos entre rotas |

---

## 🏗 Decisões de Arquitetura & Navegação

O diferencial deste projeto está na implementação da navegação no arquivo `TodoNavHost.kt`. Abaixo estão os destaques técnicos:

### 1. Type-Safe Navigation (Segurança de Tipos)
Ao invés de Strings propensas a erros, utilizamos objetos `@Serializable`. Isso garante que os argumentos passados entre telas sejam validados em tempo de compilação.

```kotlin
@Serializable object LoginRoute

// Passagem de parâmetros tipados (ex: ID opcional)
@Serializable data class AddEditRoute(val id: Long? = null)

Gestão Dinâmica de Telas (Reutilização)
A tela de Adição e Edição é a mesma (AddEditScreen). O NavHost decide o comportamento baseado na presença do id na rota:

composable<AddEditRoute> {
    // A AddEditScreen decide se busca uma tarefa no DB ou cria uma nova
    AddEditScreen(navigateBack = { navController.popBackStack() })
}

3. UX e Segurança no Backstack
Lógica para limpar o histórico ao realizar login, evitando que o botão "Voltar" do Android retorne o usuário para a tela de autenticação quando ele já está logado:

onLoginSuccess = {
    navController.navigate(ListRoute) {
        // Remove a tela de Login da pilha
        popUpTo(LoginRoute) { inclusive = true }
    }
}

Estrutura de Pastas

com.example.todolist
├── data            # Repositórios, Fontes de Dados (Room/Firebase)
├── di              # Módulos do Hilt (Injeção de Dependência)
├── navigation      # Definição de Rotas Serializáveis e NavHost
├── ui
│   ├── auth        # Telas: LoginScreen, SignUpScreen
│   ├── feature     # Telas: ListScreen, AddEditScreen
│   └── theme       # Tema: Cores, Tipografia (Material 3)

⚙️ Como Configurar e Rodar
Para executar este projeto localmente, você precisará configurar o Firebase:

Clone o repositório:

git clone [https://github.com/seu-usuario/seu-repo-todolist.git](https://github.com/seu-usuario/seu-repo-todolist.git)

Configuração do Firebase:

Acesse o Console do Firebase.

Crie um projeto e adicione um app Android com o pacote: com.example.todolist.

Habilite o método de autenticação (Email/Password).

Baixe o arquivo google-services.json.

Integração:

Mova o arquivo google-services.json baixado para a pasta app/ do projeto.

Compilação:

Abra o projeto no Android Studio Ladybug (ou superior).

Sincronize o Gradle e execute no emulador ou dispositivo físico.
