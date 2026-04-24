---
layout: default_with_comments
title: "Кружок геймдев: конспект занятия 2"
parent: "2026"
comments: true
nav_order: 2
---

<span class="text-grey-dk-000">Автор: ![Ivanmaxx](https://github.com/Ivanmaxx)</span>

<span class="text-grey-dk-000">Лицензия: Creative Commons Attribution 4.0 International</span>

# Кружок геймдев: конспект занятия 2

## Создание врага

Для начала создадим новую сцену для врага — нажмём `Ctrl+N` или `Cmd+N`. Выберем как корневой узел `CharacterBody2D` и в списке узлов переименуем в `Enemy`. Сохраним на `Ctrl+S`/`Cmd+S` как `enemy.tscn`.

Добавим к корневому узлу узел `CollisionShape2D`, в параметре `Shape` выберем `RectangleShape2D`.

![](/assets/images/guests/2026/gamedev-precis-2/1.png)

Затем слева опять выделим именно корневой узел и добавим узел `AnimatedSprite2D`. Добавим новый `SpriteFrames`. Стандартную анимацию переименуем на `run` и включим автопроигрывание — враг всегда будет двигаться к игроку.

Выберем `CollisionShape2D` и настроим его так, чтобы он был примерно такого размера:

![](/assets/images/guests/2026/gamedev-precis-2/2.png)

Перед тем, как начать скрипт врага, перейдём в скрипт игрока и в самом начала напишем `class_name Player` — это позволит взаимодействовать конкретно с игроком в скрипте врага.
Теперь создадим врагу скрипт, выбираем пустой шаблон.

```gdscript
class_name Enemy # понадобится для механики здоровья
extends CharacterBody2D


@export var player : Player # враг будет закрепляться за игроком. @export значит, что мы можем редактировать переменную в панели справа

var SPEED : float = 200.0 # скорость врага


func _physics_process(delta: float) -> void:
	var direction : Vector2 = (player.global_position - global_position).normalized() # направление движения врага - вектор, устремлённый к игроку

	velocity = direction * SPEED
	
	move_and_slide()
```

## Здоровье

Начнём создание базовой механики здоровья. В меню скриптов нажмём `Ctrl+N/Cmd+N` и назовём скрипт `health.gd`:

```gdscript
extends Node


@export var max_health : float = 100.0 # настраиваемая в инспекторе переменная максимального здоровья

var _health : float # переменная здоровья


func _ready() -> void:
	_health = max_health # при запуске игры здоровье приравнивается к максимальному
	
	
func set_health(new_health: float) -> void: # функция для изменения здоровья
	_health = new_health
	_health = clamp(_health, 0.0, max_health) # если здоровье вне допустимого, приравниваем к одной из крайностей
```
