title: State management in Angular
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->
.bottom-bar[
  {{title}}
]

---

class: impact no-counter

# {{title}}
## Mario Fernandez

---

# What is state management?

- Represent dynamic state
- Trigger changes:
* User Input
* Remote calls
- Represent in state

---


# Me

Declared _React_ fan

--

Probably unqualified to provide too many opinions on _Angular_

--

Will do so anyway

---

class: impact no-counter

# Context

---

```bash
commit 4db543c897d44849eb073facfe1562a156b02137
Author: Frontend Dev
Date:   Sun Oct 30 21:10:48 2016 +0100

    Initial commit

```

---

.center[
  ![Angular](images/angular.png)
]

---

.center[
  ![Many](images/many-people.png)
]

---

.scaled-image.center[
  ![cp](images/cp.jpg)
]

---

class: impact no-counter

# Patterns

---

class: transition

# Input/Output

---

# What

Most components do not hold their own state

--

Instead, they receive their data as `@Input`, and trigger changes upstream with `@Output`

---

## Example

```typescript
class SimpleComponent {
  @Input() selectedDate: Date;
  @Output() onJump = new EventEmitter<Date>();

  jumpBack(date: Date) {
    this.onJumpto.emit(date);
  }
}
```

---

.center[
  ![Thumbs Up](images/thumbsup.png)
]

---

class: slogan

## That's it, problem solved

---

class: slogan no-counter

## End

---

# The input train

.center[
  ![Long Chain](images/long-chain.png)
]

---

# The sideways state

.lateral-state.center[
  ![Lateral State](images/lateral-state.png)
]

---

class: transition

# ViewChild

---

class: transition

# Nested Forms

---

```html
  <!-- Parent component template -->
  <child-component
    [parentForm]="formGroup">
```

```typescript
class ChildComponent implements OnInit {
  @Input() parentForm: FormGroup
  
  ngOnInit() {
    this.parentForm.setControl('stuff', this.myControl);
  }
}
```

---

class: transition

# ngrx-store

---

# The harsh reality of introducing _Redux_

- _Redux_ is not the easiest library to explain, specially to developers without frontend background
- Incremental adoption is problematic

---

class: transition

# Data Services

---

# What

A _Service_ that:

- holds data
- offers an API to manipulate it
- provides a way to get notified of updates

---

## A simple service

```typescript
@Injectable()
export class Store {
  private timeslotSubject = new BehaviorSubject<string>(null);
  timeslot$ = this.timeslotSubject.asObservable();

  selectTimeslot(timeslot: string) {
    this.timeslotSubject.next(timeslot);
  }
}

```

---

## Changing the state

```typescript
class Publisher implements OnInit {
  constructor(private store: Store) {}
  
  onSelection(ts: string) {
    this.store.selectTimeslot(ts);
  }
}
```

---

## Getting updates

```typescript
class Subscriber implements OnInit {
  timeslot: string
  
  constructor(private store: Store) {}
  
  ngOnInit() {
    this.store.timeslot$
      .filter(ts => !!ts)
      .subscribe(ts => this.timeslot = ts)
  }
}
```

---

# Links

- https://hceris.com/angular-from-react-part1/
- https://angular.io/guide/component-interaction#parent-and-children-communicate-via-a-service
- https://blog.angular-university.io/how-to-build-angular2-apps-using-rxjs-observable-data-services-pitfalls-to-avoid/

---

class: impact no-counter

# Thank you!
## Questions?

