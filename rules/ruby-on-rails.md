# Ruby on Rails

- We follow the Rails naming conventions
- We use Rails generators for consistency
- We leverage Rails magic (associations, validations, callbacks) rather than reinventing
- We write our specs using RSpec testing conventions
- We use Rails helpers and built-in functionality before custom code

```
# Database
bin/rails db:create                     # Create database
bin/rails db:migrate                    # Run pending migrations
bin/rails db:rollback                   # Rollback last migration

# Console and server
bin/rails console                       # Start Rails console
bin/rails server                        # Start development server

# Generators
bin/rails g model NAME                  # Generate model
bin/rails g repository NAME             # Generate repository
bin/rails g use_case NAME               # Generate use-cases
bin/rails g controller NAME             # Generate controller
bin/rails g controller api/v1/NAME      # Generate api-controller
bin/rails generate views NAME           # Generate views

# Acceptance Test
bundle exec rake cucumber               # Generate Step Definitions and/ or run Acceptance Tests

# Specs
bundle exec rake specs                  # Run All Specs
bundle exec rake specs:models           # Run Model Specs
bundle exec rake specs:use_cases        # Run Use Case Specs
bundle exec rake specs:repositories     # Run Repository Specs
bundle exec rake specs:requests         # Run Controller Specs
```

## Singular or plural?

| Generator   | Form     | Example                                                |
|-------------|----------|--------------------------------------------------------|
| Controller  | Plural   | `bin/rails g controller Users`                         |
| Helper      | Plural   | `bin/rails g helper Users`                             |
| Migration   | Plural   | `bin/rails g migration AddEmailToUsers email:string`   |
| Model       | Singular | `bin/rails g model User name:string`                   |
| View        | Plural   | `bin/rails g views Users`                              |
| Use-Case    | Singular | `bin/rails g use_case User`                            |
| Repository  | Singular | `bin/rails g repository User`                          |

Step-by-step recipes (new feature, generators, localization, upgrades): the `bacon:ror-recipes` skill.
