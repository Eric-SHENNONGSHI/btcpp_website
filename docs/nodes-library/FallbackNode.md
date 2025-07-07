# Fallbacks (Controls)

This family of nodes are known as "Selector" or "Priority"
in other frameworks.

Their purpose is to try different strategies, until we find one that "works".

Currently the framework provides two kinds of nodes:

- Fallback
- ReactiveFallback

They share the following rules:

- Before ticking the first child, the node status becomes __RUNNING__.

- If a child returns __FAILURE__, the fallback ticks the next child.

- If the __last__ child returns __FAILURE__ too, all the children are halted and
 the fallback returns __FAILURE__.
 
- If a child returns __SUCCESS__, it stops and returns __SUCCESS__.
  All the children are halted. 

To understand how the two ControlNodes differ, refer to the following table:

| Type of ControlNode | Child returns RUNNING |
|---|:---:|
| Fallback | Tick again  |
| ReactiveFallback  |  Restart |

- "__Restart__" means that the entire fallback is restarted from the first 
  child of the list.

- "__Tick again__" means that the next time the fallback is ticked, the 
  same child is ticked again. Previous siblings, which returned FAILURE already,
  are not ticked again.

> Some nodes are not listed yet. See [controls](https://github.com/BehaviorTree/BehaviorTree.CPP/tree/master/include/behaviortree_cpp/controls) on Github for complete definitions.

## Fallback

In this example, we try different strategies to open the door. 
Check first (and once) if the door is open.

![FallbackNode](images/FallbackSimplified.png)

## ReactiveFallback

This ControlNode is used when we want to interrupt an __asynchronous__
child if one of the previous Conditions changes its state from 
FAILURE to SUCCESS.

In the following example, the character will sleep *up to* 8 hours. If he/she has fully rested, then the node `areYouRested?` will return SUCCESS and the asynchronous nodes `Timeout (8 hrs)` and `Sleep` will be interrupted.

![ReactiveFallback](images/ReactiveFallback.png)


 
