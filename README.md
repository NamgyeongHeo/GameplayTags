# GameplayTags
## Table of Contents
- [Intro](#Intro) 
- [Install](#Install)
- [How to use](#How-to-use)
  - [Define tags](#Define-tags)
  - [Store a tag](#Store-a-tag)
  - [Store a multiple tag](#Store-a-multiple-tag)
  - [Query with GameplayTags](#Query-with-GameplayTags)

## Intro 
`GameplayTags` provides structures that manage tags consisting of strings.

You can use these features
- Define tags with editor
- Store Tags and compare them with other tags

## Install
You need to add ScopedRegistry to `manifest.json`
```
"scopedRegistries": [
  {
    "name": "QuestionPackages", // You can change it to any name you want.
    "url": "https://registry.npmjs.org/",
    "scopes": [
      "com.question"
    ],
    "overrideBuiltIns": false
  }
]
```

Then you can add `https://github.com/NamgyeongHeo/GameplayTags.git?path=Assets/Plugins/GameplayTags` to UPM via git URL.

## How to use
### Define tags
You can define tags in `Edit>Project Settings>GameplayTags`.

Input the tag name that you want in the input field.

Check these examples.
```
// Represent character's state.
Character.State.Idle
Character.State.Walk
Character.State.Run

// Represent character's type.
Character.Type.Human
Character.Type.Dog
Character.Type.Cat

// Represent game mode.
Game.Mode.SinglePlay.StoryMode
Game.Mode.SinglePlay.TimeTrial
Game.Mode.MultiPlay.PVP
```

You can set tag name with a separator via `.` 

In this example, `Character.Type.Human` means these things.
- `Character.Type` is parent of `Character.Type.Human` tag
- `Character` is parent of `Character.Type` tag

> If you change the tags list, use `CodeGenerator>Generate` menu to update `GameplayTagsListConst` class code.

### Store a tag
`GameplayTag` can store a tag in `GameplayTagsList`

You can initialize `GameplayTag` with `GameplayTag.Create(string)` function.
```
GameplayTag idleStateTag = GameplayTag.Create(GameplayTagsListConst.Character_State_Idle);
```
Or you can also initialize `GameplayTag` in Inspector
```
[SerializeField]
GameplayTag characterTypeTag;
```

![image](https://github.com/user-attachments/assets/a40a552d-18cd-4025-b7c6-770c6fdf7b13)

### Store a multiple tag
`GameplayTagContainer` can store multiple tags.

It's similar to list of `GameplayTag` struct.
```
GameplayTagContainer availableStates = GameplayTagContainer.Create(GameplayTagsListConst.Character_State_Idle, GameplayTagsListConst.Character_State_Walk);
```
`GameplayTagContainer` can also be serialized in Inspector.

```
[SerializeField]
GameplayTagContainer selectableModes;
```
![image](https://github.com/user-attachments/assets/cccb5229-ec49-44f5-b67f-2ed95712eda7)

### Query with GameplayTags
You can write query expressions with `GameplayTag` and `GameplayTagContainer`.

`GameplayTag` can match to other `GameplayTag` instance.
```
GameplayTag characterTypeTag = GameplayTag.Create(GameplayTagsListConst.Character_Type);
GameplayTag.humanCharacterTypeTag = GameplayTag.Create(GameplayTagsListConst.Character_Type_Human);

// Match() returns true if other tag is equal or parent.
humanCharacterTypeTag.Match(characterTypeTag); // True
characterTypeTag.Match(characterTypeTag); // True
characterTypeTag.Match(humanCharacterTypeTag); // False

// MatchExact() returns true if other tag is equal.
humanCharacterTypeTag.MatchExact(humanCharacterTypeTag); // True
humanCharacterTypeTag.MatchExact(characterTypeTag); // False
```

`GameplayTagContainer` can match with multiple `GameplayTag` instances.
```
GameplayTagContainer selectableModes = GameplayTagContainer.Create(GameplayTagsListConst.Game_Mode_SinglePlay_StoryMode, GameplayTagsListConst.Game_Mode_SinglePlay_TimeTrial);
GameplayTag gameModeTag = GameplayTag.Create(GameplayTagsListConst.Game_Mode);
GameplayTag storyModeTag = GameplayTag.Create(GameplayTagsListConst.Game_Mode_SinglePlay_StoryMode);

// HasTag() returns true if any tag's Match() return true.
selectableModes.HasTag(gameModeTag); // True
selectableModes.HasTag(storyModeTag); // True

// HasTag() returns true if any tag's MatchExact() return true.
selectableModes.HasTagExact(gameModeTag); // False
selectableModes.HasTagExact(storyModeTag); // True
```
