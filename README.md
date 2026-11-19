# Simple Vault — задание 8: каркас Liquidity Vault

Учебный Solana-программа на Anchor 0.31.1. Эта версия архива содержит **только реализацию недели 8**.

## Что реализовано

1. `initialize_vault` — администратор создаёт базовую структуру vault:
   - `VaultState` PDA — настройки и состояние хранилища;
   - `vault_token_account` PDA — SPL token account, принадлежащий vault PDA;
   - `share_mint` PDA — mint для share-токенов с 6 decimals, authority которого является vault PDA.
2. `create_user_card` — пользователь создаёт личную `UserCard` PDA.
   - Seeds: `["user_card", vault_state, user]`;
   - карточка привязана к конкретному vault и владельцу;
   - один пользователь может создать только одну карточку в рамках одного vault.
3. Тесты проверяют создание vault, владельца token account, параметры share mint, создание двух отдельных карточек и запрет повторного создания карточки.

## Что намеренно НЕ реализовано в этой версии

- `deposit` и выпуск share-токенов пользователю — неделя 9;
- `withdraw`, `burn`, CPI transfer с подписью PDA — неделя 10.

Share mint создаётся на неделе 8, но его выпуск/сжигание токенов будет добавлен на следующих этапах.

## Структура

```text
vault-dev-students-vault-card/
├── programs/
│   └── simple_vault/
│       ├── Cargo.toml
│       └── src/
│           └── lib.rs
├── tests/
│   └── week8.ts
├── Anchor.toml
├── Cargo.toml
├── package.json
└── README.md
```

## Требования

- Rust stable
- Solana CLI 1.18+
- Anchor CLI 0.31.1
- Node.js 18+
- Yarn

## Установка и сборка

```bash
yarn install
anchor build
```

Если используешь новый program keypair, синхронизируй program ID и пересобери:

```bash
anchor keys sync
anchor build
```

## Настройка devnet и запуск

В `Anchor.toml` по умолчанию указан devnet и кошелёк `~/.config/solana/id.json`.

```bash
solana config set --url devnet
solana address
solana balance
```

Для запуска тестов программа должна быть задеплоена в devnet:

```bash
anchor deploy --provider.cluster devnet
yarn test
```

Тесты используют SOL администратора для финансирования тестовых пользователей. Убедись, что у кошелька достаточно SOL и что он настроен на тот же devnet.

## Ожидаемые тесты недели 8

- `initialize_vault`: настройки vault, token account и share mint созданы корректно;
- `create_user_card`: у двух пользователей разные PDA-карточки;
- `create_user_card`: повторное создание карточки одним пользователем отклоняется.
