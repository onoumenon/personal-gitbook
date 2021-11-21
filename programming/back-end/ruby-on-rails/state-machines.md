# State machines

aasm gem: [https://github.com/aasm/aasm](https://github.com/aasm/aasm)

after commit: [https://github.com/Envek/after\_commit\_everywhere](https://github.com/Envek/after\_commit\_everywhere)



To add a status to a model:

Add 'status' column, include aasm, add aasm function

```
class Job
  include AASM

  aasm do
    state :sleeping, initial: true
    state :running, :cleaning

    event :run do
      transitions from: :sleeping, to: :running
    end

    event :clean do
      transitions from: :running, to: :cleaning
    end

    event :sleep do
      transitions from: [:running, :cleaning], to: :sleeping
    end
  end

end
```

This provides you with a couple of public methods for instances of the class `Job`:

```
job = Job.new
job.sleeping? # => true
job.may_run?  # => true
job.run
job.running?  # => true
job.sleeping? # => false
job.may_run?  # => false
job.run       # => raises AASM::InvalidTransition
```

Then you can call these methods from your services

You can combine it with after commit:

```
event :completed, after_commit: :notify_completion do
    transitions from: :processing, to: :converted, guard: :conversion_completed?
end
```

