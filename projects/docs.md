
# Table of Contents

1.  [Components](#orgd0fcd73)
    1.  [DiceRoller](#org1b21507)
        1.  [props:](#orgcd7693b)
    2.  [ForceTokens](#org9e27c83)
        1.  [props:](#org102aead)
    3.  [DicePool](#orgef7c2e2)
        1.  [props:](#org2aeab74)
        2.  [state:](#org5c4d08e)
        3.  [behavior:](#org1f5479a)
    4.  [Dice](#org077b0ec)
    5.  [Results](#org25ed61d)
        1.  [props](#org632f74d)
2.  [Store](#orge94f0d7)
3.  [Diagrams](#org7eb66b9)



<a id="orgd0fcd73"></a>

# Components


<a id="org1b21507"></a>

## DiceRoller


<a id="orgcd7693b"></a>

### props:

-   diceTypes[] \*default to diceTypes.Genesys
-   roll() \*fromStore


<a id="org9e27c83"></a>

## ForceTokens


<a id="org102aead"></a>

### props:

-   results[] \*from store => filters locally for force dice results


<a id="orgef7c2e2"></a>

## DicePool


<a id="org2aeab74"></a>

### props:

-   diceTypes[]
-   roll


<a id="org5c4d08e"></a>

### state:

-   dicePool{} \*mapping of diceType:amount


<a id="org1f5479a"></a>

### behavior:

-   clear: clears dicePool to 0&rsquo;s
-   roll: calls roll prop


<a id="org077b0ec"></a>

## Dice


<a id="org25ed61d"></a>

## Results


<a id="org632f74d"></a>

### props


<a id="orge94f0d7"></a>

# Store

results[ ]

Actions:
    add-results

ActionCreators:
    addResult

Reducers:
    results[ ],action => results[ ]


<a id="org7eb66b9"></a>

# Diagrams

![img](./assets/components.png)

