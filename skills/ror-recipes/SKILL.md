---
name: ror-recipes
description: Step-by-step Ruby on Rails recipes for this codebase - create a feature end to end (feature file, model, repository, use-case, controllers, views), build a custom generator, add static or Active Record localization, and upgrade Ruby, Rails or gems. Use when doing one of those tasks; day-to-day conventions and commands live in the ruby-on-rails rule.
---

# Ruby on Rails - Recipes

Conventions and the command cheat sheet are in the `ruby-on-rails` rule. This skill holds the longer walkthroughs.

## Building Generators

- Create a new generator, e.g. `rails g generator use_case`
- Generators are created at `lib/generators`
- Fill in the `USAGE`-file
- Add the template-files (use the `.rb.tt`-file-extension)
- Implement the Generator (e.g. `use_case_generator.rb`) file

--

## Project Updating

### Update gems
- Update `Gemfile`
- Re-run `bundle install`

### Update ruby-lang
```shell
rbenv install 3.2.1
rbenv global 3.2.1
rbenv uninstall 3.0.1
```
- Update version(s) in `.ruby-version`
- Update version(s) in `.github/workflows` / github.com variables
- Update version(s) in `Readme.md`
- Run `bundle install`

### Update rails
- Update version(s) in `Gemfile`
- Update version(s) in `Readme.md`
- Run `bundle install`
- Run `rails app:update`

--

## Create a Feature

### Feature definition
- Write/ extend the `.feature`
- Run `bundle exec rake cucumber`. This will generate the new step definitions.
- Copy this snippet to the step definitions file
- Fill out the steps
- Run `bundle exec rake cucumber` again. All new tests should fail.

### Feature classes
#### Model
- `bin/rails g model Restaurant name:string description:text availability:boolean dish:belongs_to translations:jsonb`
- Open the db-migration file and add default values for `translations` and `boolean`-values, e.g.:
    ```
    t.boolean :availability, null: true, default: true
    t.jsonb :translations, default: {}
    ```
- Setup relations if necessary:
    - e.g. `bin/rails g migration AddRestaurantForeignKeyToMenus`
    - [https://guides.rubyonrails.org/association_basics.html](https://guides.rubyonrails.org/association_basics.html)
    - [https://human-se.github.io/rails-demos-n-deets-2021/demos/one-to-many-associations/](https://human-se.github.io/rails-demos-n-deets-2021/demos/one-to-many-associations/)
- Migrate the DBs:
    - `bin/rails db:migrate RAILS_ENV=test`
    - `bin/rails db:migrate RAILS_ENV=development`
- Prefill the factory
- Create the model specs
- Run `bundle exec rake specs:models`. All new specs (and eventual related ones) should fail.
- Implement the model class

#### Repository
- `bin/rails g repository Restaurant`
- Add the new Repository to the Resolver `app/resolvers/<app>_resolver.rb`
- Create the repository specs
- Implementation should be fine in 99% of all cases already

#### Use-Cases
- `bin/rails g use_case Restaurant`
- Create the use-case specs
- Implementation should be fine but still needs `strong_params`-method to be filled out 

#### Controller + Views
- `bin/rails g controller Contracts`
- Create the controller specs
- `bin/rails g controller api/v1/contracts`
- Create the API controller specs
- Create the schema file(s) in `spec/support/api/v1/schemas/`
- `bin/rails generate views Restaurants`
- Create the views for web and API
- Add the new controllers to `config/routes.rb`
- Implement the controllers

--

## Localization

### Static localization

- In `config/application.rb` add your locales
- In `config/locales/en.yml` add your translations
- In the views replace hard-coded strings with e.g. `<%= t('.title') %>`

### Active Record localization

- Create a migration to add the `translations`-attribute to the model: `bin/rails generate migration AddTranslationsToAllergens translations:jsonb`
- In the migration file add a default value `{}`, e.g.: `add_column :allergens, :translations, :jsonb, default: {}`
- Migrate your databases: `bin/rails db:migrate RAILS_ENV=test`, `bin/rails db:migrate RAILS_ENV=development`, etc.
- In the model class add the following:
    ```
    extend Mobility
    translates :name
    translates ...
    ```
- Update the forms to iterate through all available languages:
    ```
    <% I18n.available_locales.each do |locale| %>
    	<div>
        <% name = "name_#{Mobility.normalize_locale(locale)}" %>
        <%= form.label name %><br>
        <%= form.text_field name %>
    		<%= allergen.errors.full_messages_for(name).each do |message| %>
    		<div><%= message %></div>
		<% end %>
    	</div>
    <% end %>
    ```
- Extend the create- and edit-use-cases to permit the generated parameters `..._en, ..._de, etc.`:
    ```
    translated_params = %w[name ...].map { |field| available_locales_param(field) }.flatten
    params.require(:allergen).permit(:name, translated_params)
    ```
    
- Add `json.translations allergen.translations` to the API-`jbuilder`-file
- Update json schemas and add `translation`-object
