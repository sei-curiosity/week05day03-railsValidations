[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Rails Model Validations

## Objectives

By the end of this lesson you should be able to...

- Know, what is validation and why we need it
- Add validations to our models

## Introduction

Active Record allows you to validate the state of a model before it gets written
into the database. There are several methods that you can use to check your
models and validate that an attribute value is not empty, is unique and not
already in the database, follows a specific format and many more.

Validation is a very important issue to consider when persisting to database, so
the methods `create`, `save` and `update` take it into account when
running: they return `false` when validation fails and they didn't actually
perform any operation on database. All of these have a bang counterpart (that
is, `create!`, `save!` and `update!`), which are stricter in that
they raise the exception `ActiveRecord::RecordInvalid` if validation fails.

Let's look at how we might validate some things.

-   **Empty Values**

      To prevent empty values we'll use `validates <property>, presence: true`.
      This is a slightly more restrictive check than `null: false`;
       it disallows both empty values (`nil` in Ruby, `NULL` in the database),
       and it also disallows empty strings (for string properties).

      **EXAMPLE :**
      To set an empty-value validator in our Artist model, you might write

      ```ruby
      class Artist < ActiveRecord::Base
        has_many :songs
        validates :name, presence: true
      end
      ```

-   **Uniqueness**

      To ensure that a property is unique,
       we'll use `validates <property>, uniqueness: true`.

      **EXAMPLE :**
      To set some uniqueness validators in my Song model, you might write

      ```ruby
      class Song < ActiveRecord::Base
        belongs_to :artist
        validates :title, uniqueness: true
      end
      ```

-   **References**

      For referential integrity checks,
       we'll use `validates <model>, presence: true`,
       where `<model>` is the symbol we passed to `belongs_to`.

      **EXAMPLE :**
       You want to test that a Song instance refers to
       an instance of Artist ;
       in that case, you might write the following:

      ```ruby
      class Song < ActiveRecord::Base
        belongs_to :artist

        validates :artist, presence: true
      end
      ```

`ActiveRecord::Base` comes with a slew of other validators we can use, as well as the mechanisms to create our own custom validators.


A quick example to illustrate:

```ruby
class Song < ActiveRecord::Base
  validates :title, presence: true
end

song = Song.new(title: "Its a good day for Ruby")
song.valid?  # => checks validations and returns a boolean
song.save    # => rollback db
song.save!   # => specifies failure

song.title = "Its a good day for Ruby"
song.valid?
song.save
```

You can learn more about validations in the [Active Record Validations
guide](http://guides.rubyonrails.org/active_record_validations.html).

#### Validate Song Model

We set our [validations](http://guides.rubyonrails.org/active_record_validations.html) in our app/models/song.rb model.

Let's make sure each song definitely has a title and a genre before they're saved.

```ruby
class Song < ActiveRecord::Base
  validates :title, presence: true
  validates :genre, presence: true
end
```

* Type `reload!` into the console to update your model validations.
* Try saving a song with no title and see what error is thrown.
* Try calling `valid?` on a new person.
* Walk through the Rails Docs and show that you can add error messages to your validations (e.g.- `too_short: "must have at least %{count} words"`).

**WE DO:**

Add the following validation to `song.rb`:

```ruby
validates :title, :genre, length: { minimum: 3, too_short: "must have at least %{count} words" }
```
(if numeric input is available)
or add `numericality` to the albums field:

```ruby
validates :albums, numericality: true
```

**YOU DO:**

- Create a new `Song` table column named `year` and add a validation. 
- Attempt to create a new `Song` and make it fail. 
- Make the new `Song` pass.


<br>

## Migrations/Validations Exercise

[Can be found here](rails_active_record_migrations-and-validations_exercise.md)

## Further Reading

* [Active Record Overview - Rails Guides](http://guides.rubyonrails.org/active_record_basics.html)
* [Active Record Migrations - Rails Guides](http://edgeguides.rubyonrails.org/active_record_migrations.html)
* [Active Record Associations - Rails Guides](http://guides.rubyonrails.org/association_basics.html)
* [Active Record Validations - Rails Guides](http://guides.rubyonrails.org/active_record_validations.html)
