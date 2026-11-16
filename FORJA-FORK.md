# Fork da Forja — `forja`

Este é um fork de `tangency-labs/fieldcraft` mantido pela plataforma **Forja**
(`dionarley/forja`). O `fieldcraft` é a bancada de **verificação** da plataforma:
malha hex/tet, FEA implícito e explícito, calor e CFD por lattice Boltzmann, com
cada resultado conferido contra a teoria.

É o elo que fecha a cadeia do produto: ele abre um design `.solidcraft` e
reconstrói os corpos pela linha do tempo, então a peça paramétrica do Brasa
(`PS-001`) entra na análise sem exportar STEP no meio.

A branch `forja` carrega as mudanças locais. **Não abra PR na origem.**

## Como a Forja usa

Submodule em `apps/fieldcraft`. É **app, não biblioteca**: tem workspace próprio
(`edition 2024`, rust 1.95) e fica fora do workspace do `forja-pdm` e do build do
`tools/build.sh`. Compile dentro do submodule, sob demanda:

```sh
cd apps/fieldcraft
cargo build --locked -p fieldcraft -p fieldcraft-cli
```

O `rust-toolchain.toml` pina **Rust 1.99.1**: use rustup (é ele que instala a
versão sozinho). O `rustc` do sistema nesta máquina é 1.96.1 e não compila o
workspace. `pcbcraft` não tem `rust-toolchain.toml` e pede 1.95 ou mais novo.

O agente fala com ele por MCP: `fieldcraft-cli mcp` roda sem janela;
`fieldcraft --control 7070 &` mais `fieldcraft-cli mcp --connect 127.0.0.1:7070`
prende a sessão ao app aberto, para a pessoa ver a análise enquanto acontece.

## O que não fazer

- Não reformate nem renomeie código de terceiro. O patch tem que ser mínimo e
  explicado aqui.
- Não versione resultado de análise (malhas, decks, campos) no submodule: são
  artefatos, vão para o TeamCraft ou para `brasa/out/`.

## Atualizar a partir da origem

```sh
git remote add upstream https://github.com/tangency-labs/fieldcraft.git   # uma vez
git fetch upstream
git checkout forja
git rebase upstream/main
git push --force-with-lease origin forja
```
