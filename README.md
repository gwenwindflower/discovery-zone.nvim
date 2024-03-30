# Discovery Zone

Discovery zone is a Neovim playground for exploring the dbt Cloud GraphQL APIs, specifically the Discovery API, although it's one line to cut over to the Semantic Layer API.

It aims to provide a rich development environment for writing and testing dbt GraphQL queries, with the following features:

- Language server for GraphQL including schema autocompletion
- Instructions for configuring syntax highlighting
- An easy command to run queries in the terminal
- A repository of useful queries

## How to use

The `.graphqlrc.yml` is the configuration for the GraphQL plugin, which is this project's primary dependency. This in turns powers the `graphql-language-service-cli` which provides the language server for the GraphQL filetype. These are included as dependencies in the `package.json`, although the `graphql-language-service-cli` is not a bad idea to install globally, as you may want to configure it for other projects.

I like `bun` as a TS package manager, but you can use `npm`, `pnpm`, or `yarn` as you see fit.

### Install dependencies

```bash
bun i
```

### Configure LSP

I recommend using nvim-lspconfig to configure the LSP. You don't need any specific configuration, just to add it as a server. Here's an example configuration for the GraphQL language server in vanilla Neovim lua config:

```lua
require'lspconfig'.graphql.setup{}
```

If you're using LazyVim, as I am (which I so highly recommend), it looks like this:

```lua
{
  "neovim/nvim-lspconfig",
  opts = {
    servers = {
      -- I don't like markdownlint so can't use the LazyVim Extras
      -- thus manually adding this
      graphql = {},
    },
  },
},
```

### Enable TreeSitter syntax highlighting

If you have TreeSitter enabled, you can get syntax highlighting for GraphQL queries. Issue this command to add the grammar `:TSInstall graphql`. If you want to, as you probably should, enable it by default, you can add the following to your config with LazyVim:

```lua
{
  "nvim-treesitter/nvim-treesitter",
  opts = {
    ensure_installed = "graphql",
  },
},
```

### Set an environment variable with your authentication token

By default it looks for an environment variable called `DBT_CLOUD_DISCOVERY_API_KEY`.

### Install `httpie`

HTTPie is a great tool for making API requests from the command line. I find it to have easier to read syntax than `curl` especially for this kind of thing. You can install it with Homebrew:

```bash
brew install httpie
```

After all that you should be set up!

## Running queries

Queries rely on two files per query: the query file and its corresponding variables file. They are expected to have the same name, with variables being called `[name]-variables.json` for a `[name].graphql` file.

Pass the root `[name]` as an environment variable to the `query` script and everything should Just Work:

```bash
QUERY=[name] bun query
```
