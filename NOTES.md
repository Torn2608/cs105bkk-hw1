# HW1 notes

Answer each question in a few sentences. Be specific: name the function, the error or the command.

## What did you try?

I wrote this homework with help from Claude Code (an AI coding assistant). First I ran `python -m pytest` to see all 14 tests fail with `NotImplementedError`. Then I filled in the parts in the README's order: `MenuItem`, `PromoItem.cost`, the `Order` methods and `cheapest`. After the 14 visible tests passed, I wrote a small script to try the cases the README says grading also checks: a price of 0 or below keeps the old price, `PromoItem.cost` for 0, 1, 2, 3, 6 and 7 items, an empty order costs 0, food at exactly 299 and 300 baht, two orders made from the same empty list, and `cheapest` with a tie and with a non-`Order` object that only has `total()`.

## What broke, and how did you fix it?

The main trap was the aliasing in `Order.__init__`. Writing `lines=[]` as the default, or `self.lines = lines or []`, looks fine, but the first one shares one list between all orders. The second one keeps the caller's list when they pass an empty `[]`, because an empty list is falsy. I fixed it by checking `if lines is None` and otherwise storing `list(lines)`, which is a new copy. In `__init__` I also had to write `self.price = price` and not `self._price`, or the setter's `ValueError` check would be skipped. My first version of `cheapest` was a loop that called `total()` again every time it compared. I replaced it with `min(options, key=..., default=None)`, which returns `None` for an empty list and keeps the first option when two totals tie.

## What would you do differently next time?

I would write the edge-case checks, like the 300-baht limit, a quantity of 0 and the empty list, as my own pytest tests before writing the code, not as a script afterwards. Then they would run every time with `python -m pytest`. I would also write more of the code myself before asking an AI for help, so I practise the Session 1 to 3 ideas properly.
