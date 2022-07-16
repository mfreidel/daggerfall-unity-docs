# legal

> Changes the player's legal repute

```
legal repute nn
```

This action modifies the legal standing of the PC in the current province, or the province where the quest containing this action was obtained. Positive values increase the PC's repute, while negative numbers decrease it. The change is relative to the PC's present legal standing in the province, not an absolute state.

```
_failedQuest_ task:
    legal repute -15
```

In this quest fragment, if the PC fails to meet the goals of the quest, his repute in the province loses 15 points.
