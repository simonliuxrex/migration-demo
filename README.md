## What you will learn
- A source controlled method of schema management
- A way to migrate from non-source controlled schema.
- Limitation and gotchas
- Alternative tools

## Rollback/forward
```
rails g migration add_email_to_users
add_column :users, :email, :string, null: false
rails db:migrate
rails db:rollback
add_index :users, :email, unique: true
```
see `schema_migrations` table

## Up/down
```
  def up
    change_table :products do |t|
      t.change :price, :string
    end
  end

  def down
    change_table :products do |t|
      t.change :price, :integer
    end
  end

  #######################################

  def up
    execute 'CREATE PROCEDURE CalcIncome...'
  end

  def down
    execute 'DROP PROCEDURE CalcIncome;'
  end
```

## Schema dump
```
CREATE TABLE `books` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `created_at` datetime(6) NOT NULL,
  `updated_at` datetime(6) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `index_users_on_email` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

rails db:schema:dump
```

## Limitations
1. Synchronous migration
  - Could take a long time if you are using shards on production.
2. `rails db:schema:dump` will not dump procedures or functions.

## Things to lookout for
1. Always take a db snapshot before performing a production schema migration.
2. Have a rough idea of how much time each migration will take, prior to migration.
  - You can measure migration time by using recent snapshots.
3. Do not modify past migration on production branch.
4. Rollback/forward only for development.

## Alternative tools
 - https://github.com/amacneil/dbmate This one looks like the ONE!
 - https://github.com/golang-migrate/migrate
 - https://flywaydb.org/
 - https://www.liquibase.org/

## Supplement readings
 - https://edgeguides.rubyonrails.org/active_record_migrations.html
