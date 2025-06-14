# Core Models

This document highlights the main domain models used throughout the application and how they relate to one another.

## Family

*Top level entity that owns accounts, users and data.*

**Associations**

- `has_many :users`
- `has_many :accounts`
- `has_many :budgets`
- `has_many :categories`
- `has_many :transactions, through: :accounts`

**Notable methods**

- `syncing?` – checks if any accounts or items are currently syncing.

## Account

*Represents a financial account such as a bank or brokerage account.*

**Associations**

- `belongs_to :family`
- `has_many :entries`
- `has_many :transactions, through: :entries`
- `has_many :balances`

**Notable methods**

- `self.create_and_sync` – builds an account with initial balance entries and immediately queues a sync job.

## Transaction

*Represents a single inflow or outflow on an account.*

**Associations**

- `belongs_to :category`
- `belongs_to :merchant`
- `has_one :entry` (via `Entryable`)
- `has_many :tags` (through taggings)

**Notable methods**

- `Transaction::Search` – utility object that builds an ActiveRecord query from search parameters.

## Budget

*Monthly budget for a family.*

**Associations**

- `belongs_to :family`
- `has_many :budget_categories`

**Notable methods**

- `sync_budget_categories` – ensures the budget has a category for each of the family’s expense categories.

## Typical Workflow

1. **Create a Family** – a user signs up and a `Family` record is created.
2. **Add Accounts** – accounts are added using `Account.create_and_sync`, which seeds initial balance entries and begins a sync with any connected provider.
3. **Record Transactions** – transactions arrive via account syncs or manual entry. They can be searched using `Transaction::Search` and categorized as needed.
4. **Manage Budgets** – each month a `Budget` is created (often via `Budget.find_or_bootstrap`). `sync_budget_categories` keeps its categories in sync with the family’s list so spending can be tracked against budgeted amounts.
