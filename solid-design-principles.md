### SOLID Design Principles - Useful principles of object-oriented design

- Set of design principles introduced by Robert C. Martin (also known as Uncle Bob)
- Design principles are frequently referred to in design pattern literature.


#### 1) Single Responsibility Principle (SRP) / Separation of Concerns (SOC)

- If you have a Class, then that Class should have it's primary responsibility, and it should not take up other responsibilities. 
- A class should have a single reason to change, and that change should somehow be related to it's primary responsibility.
- As per wikipedia - The single-responsibility principle: "There should never be more than one reason for a class to change." In other words, every class should have only one responsibility.
 
 ```
 class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)

j = Journal()
j.add_entry("I cried today.")
j.add_entry("I ate a bug.")
print(f"Journal entries:\n{j}\n")
```

So, far the above code won't break SRP.

Now, if we wish to add the functionality of save and load to the journal, then Journal would be taking additional responsibility of persistence.

```
# Breaking SRP

class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)

    # break SRP
    def save(self, filename):
        file = open(filename, "w")
        file.write(str(self))
        file.close()

    def load(self, filename):
        pass

    def load_from_web(self, uri):
        pass
        
j = Journal()
j.add_entry("I cried today.")
j.add_entry("I ate a bug.")
print(f"Journal entries:\n{j}\n")
```

If we have other applications apart from Journal, then this functionality of persistence, might be required centrally and can be modified easily, if separated to suit the needs arising therein.
Also, by keeping it separate from the application, we wouldn't have to modify the class of each and every application, in case persistence functionality changes in any way.

**Better implementation -**

```
class Journal:
    def __init__(self):
        self.entries = []
        self.count = 0

    def add_entry(self, text):
        self.entries.append(f"{self.count}: {text}")
        self.count += 1

    def remove_entry(self, pos):
        del self.entries[pos]

    def __str__(self):
        return "\n".join(self.entries)

    # break SRP
    def save(self, filename):
        file = open(filename, "w")
        file.write(str(self))
        file.close()

    def load(self, filename):
        pass

    def load_from_web(self, uri):
        pass


class PersistenceManager:
    def save_to_file(journal, filename):
        file = open(filename, "w")
        file.write(str(journal))
        file.close()


j = Journal()
j.add_entry("I cried today.")
j.add_entry("I ate a bug.")
print(f"Journal entries:\n{j}\n")

p = PersistenceManager()
file = r'c:\temp\journal.txt'
p.save_to_file(j, file)

# verify!
with open(file) as fh:
    print(fh.read())
```

- So, according to SRP, we do not want to overload our object with too many responsibilities
- We have something like **anti-pattern** (which is opposite of pattern). **Anti-patterns are undesirable**. One such anti-pattern is **"God Object"**. Some newbie developers, often put up everything in the kitchen sink in one class. So, **Single Responsibility Principle, prevents us from creating "God Object"**.


