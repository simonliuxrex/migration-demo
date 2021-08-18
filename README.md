## Rollback/forward
```
rails g migration add_email_to_users
add_column :users, :email, :string, null: false
rails db:migrate
rails db:rollback
add_index :users, :email, unique: true

```
see `scheema_migrations`

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
  - Will take a long time if you are using shards.
2. `rails db:schema:dump` will not dump procedures or functions.

## Things to lookout for
1. Always take a db snapshot before migration.
2. Have a rough idea of how much time each migration will take.
  - You can measure migration time by using recent snapshots.

## Alternative tools
 - https://github.com/golang-migrate/migrate
 - Flyway
 - LiquiBase

## Supplement readings
 - https://edgeguides.rubyonrails.org/active_record_migrations.html
