# Testing en Python

## unittest

```python
import unittest

class TestPlayer(unittest.TestCase):
    def test_level_up(self):
        player = Player("Test", 1)
        player.level_up()
        self.assertEqual(player.level, 2)
```

---

## Ejercicio

Crea tests para la clase Player.