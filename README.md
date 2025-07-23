# Java Reactive Programming

## Tabla de contenido

- [Introducción a la programación reactiva](#introduction-reactive)
  - [Antecedentes](#antecedentes)
    - [Problemas de la programación imperativa/síncrona](#problemas-imperativa-sincrona)
    - [Problemas de la programación asíncrona tradicional](#problemas-asincrona-tradicional)
  - [Process / Thread / CPU / RAM / Scheduler](#process-thread-cpu-ram-scheduler)
    - [Repaso de Hilos y Procesos](#hilos-procesos-reactive)
    - [CPU y RAM](#cpu-ram-reactive)
    - [Scheduler del Sistema Operativo](#scheduler-reactive)
    - [Concurrencia vs. Paralelismo](#concurrencia-paralelismo-reactive)
    - [Efectos secundarios de la concurrencia](#efectos-secundarios-concurrencia-reactive)
  - [IO Models](#io-models)
    - [I/O Bloqueante (Blocking I/O)](#io-bloqueante)
    - [I/O No Bloqueante (Non-Blocking I/O)](#io-no-bloqueante)
    - [I/O Asíncrono (Asynchronous I/O)](#io-asincrono)
    - [Java NIO y NIO.2 (AIO)](#java-nio-nio2)
  - [Communication Patterns](#communication-patterns)
    - [Síncrono Request/Response](#sincrono-request-response)
    - [Asíncrono Request/Response](#asincrono-request-response)
    - [Streaming](#streaming-reactive)
    - [Event-Driven Architectures](#event-driven-architectures)
  - [¿Qué es la programación reactiva?](#what-is-reactive-programming)
    - [Definición formal](#definicion-formal-reactive)
    - [Programación Orientada a Eventos vs. Reactiva](#orientada-eventos-vs-reactiva)
    - [Ventajas del enfoque reactivo](#ventajas-enfoque-reactivo)
  - [Reactive Streams Specification](#reactive-streams-specification)
    - [Orígenes y propósito](#origenes-proposito-reactive-streams)
    - [Interfaces Clave](#interfaces-clave-reactive-streams)
    - [Reglas y Garantías de la Especificación](#reglas-garantias-reactive-streams)
  - [Publisher/Subscriber Communication - Step By Step](#publisher-subscriber-communication)
    - [Demostración del flujo](#demostracion-flujo-reactive)
    - [El rol de `request()` (Backpressure)](#rol-request-backpressure)
    - [Diagramas Marble (Marble Diagrams)](#diagramas-marble)

- [Mono](#mono)
  - [Concepto de `Mono`](#concepto-mono)
    - [Emisión de 0 o 1 elemento](#emision-0-o-1-elemento)
    - [Lazy vs. Eager](#lazy-vs-eager-mono)
  - [Creación de `Mono`s](#creacion-monos)
    - [`Mono.just()` y `Mono.empty()`](#mono-just-empty)
    - [`Mono.error()`](#mono-error)
    - [`Mono.fromCallable()`, `Mono.fromRunnable()`, `Mono.fromSupplier()`](#mono-fromcallable-runnable-supplier)
    - [`Mono.defer()`](#mono-defer)
    - [`Mono.fromFuture()`, `Mono.fromCompletionStage()`](#mono-fromfuture-completionstage)
  - [Transformación de `Mono`s](#transformacion-monos)
    - [`map()`](#map-mono)
    - [`flatMap()`](#flatmap-mono)
    - [`flatMapMany()`](#flatmapmany-mono)
  - [Manejo de Errores en `Mono`](#manejo-errores-mono)
    - [`onErrorReturn()`](#onerrorreturn-mono)
    - [`onErrorResume()`](#onerrorresume-mono)
    - [`doOnError()`](#doonerror-mono)
    - [`retry()`](#retry-mono)
    - [`doFinally()`, `doOnTerminate()`](#dofinally-doonterminate-mono)
  - [Manejo de Eventos del Ciclo de Vida](#manejo-eventos-ciclo-vida-mono)
  - [Operadores de Condición y Filtrado](#operadores-condicion-filtrado-mono)
  - [Combinación de `Mono`s](#combinacion-monos)
    - [`zipWith()`](#zipwith-mono)
    - [`and()`](#and-mono)
    - [`then()`](#then-mono)
  - [Suscripción a un `Mono`](#suscripcion-mono)
    - [`subscribe()`](#subscribe-mono)
    - [`block()`](#block-mono)
    - [`toFuture()`, `toCompletionStage()`](#tofuture-tocompletionstage-mono)

- [Flux](#flux)
  - [Concepto de `Flux`](#concepto-flux)
    - [Emisión de 0 a N elementos](#emision-0-a-n-elementos)
    - [Lazy Execution](#lazy-execution-flux)
  - [Creación de `Flux`s](#creacion-fluxs)
    - [`Flux.just()`](#flux-just)
    - [`Flux.fromIterable()`, `Flux.fromArray()`, `Flux.fromStream()`](#flux-fromiterable-array-stream)
    - [`Flux.range()`](#flux-range)
    - [`Flux.interval()`](#flux-interval)
    - [`Flux.generate()`](#flux-generate)
    - [`Flux.create()`](#flux-create)
    - [`Flux.error()`, `Flux.empty()`](#flux-error-empty)
  - [Transformación de `Flux`s](#transformacion-fluxs)
    - [`map()`](#map-flux)
    - [`flatMap()`](#flatmap-flux)
    - [`concatMap()`](#concatmap-flux)
    - [`groupBy()`](#groupby-flux)
  - [Filtrado y Condición](#filtrado-condicion-flux)
  - [Combinación de `Flux`s](#combinacion-fluxs)
    - [`merge()`](#merge-flux)
    - [`concat()`](#concat-flux)
    - [`zip()`](#zip-flux)
    - [`combineLatest()`](#combinelatest-flux)
    - [`withLatestFrom()`](#withlatestfrom-flux)
  - [Agregación y Reducción](#agregacion-reduccion-flux)
    - [`reduce()`](#reduce-flux)
    - [`collectList()`, `collectMap()`, `collectMultimap()`](#collectlist-map-multimap-flux)
    - [`count()`, `hasElements()`](#count-haselements-flux)
  - [Manejo de Errores en `Flux`](#manejo-errores-flux)
  - [Suscripción a un `Flux`](#suscripcion-flux)
    - [`subscribe()`](#subscribe-flux)
    - [`blockFirst()`, `blockLast()`](#blockfirst-blocklast-flux)

- [Operators](#operators)
  - [Categorías de Operadores](#categorias-operadores)
  - [Profundización en Operadores Comunes](#profundizacion-operadores-comunes)
  - [Operadores de Error Handling](#operadores-error-handling-reactive)
    - [`retryWhen()`](#retrywhen-reactive)
    - [`using()`](#using-reactive)
  - [Backpressure y Operadores](#backpressure-operadores)
  - [Composición de Operadores](#composicion-operadores)

- [Hot & Cold Publishers](#hot-cold-publishers)
  - [Publishers Fríos (Cold Publishers)](#cold-publishers)
    - [Definición](#definicion-cold-publishers)
    - [Analogía](#analogia-cold-publishers)
    - [Ejemplos](#ejemplos-cold-publishers)
    - [Características](#caracteristicas-cold-publishers)
  - [Publishers Calientes (Hot Publishers)](#hot-publishers)
    - [Definición](#definicion-hot-publishers)
    - [Analogía](#analogia-hot-publishers)
    - [Ejemplos](#ejemplos-hot-publishers)
    - [Características](#caracteristicas-hot-publishers)
  - [Transformar Cold a Hot](#transformar-cold-a-hot)
    - [`publish()` / `autoConnect()`](#publish-autoconnect)
    - [`share()`](#share-reactive)
    - [`cache()`](#cache-reactive)
  - [Uso de `Sinks`](#uso-sinks)
    - [`Sinks.One`, `Sinks.Many`, `Sinks.Empty`](#sinks-one-many-empty)
    - [Cómo usar `Sinks`](#como-usar-sinks)

- [Threading & Schedulers](#threading-schedulers)
  - [El modelo de hilo de Project Reactor](#modelo-hilo-project-reactor)
    - [`subscribeOn()`](#subscribeon)
    - [`publishOn()`](#publishon)
  - [Schedulers de Project Reactor](#schedulers-project-reactor)
    - [Concepto](#concepto-schedulers)
    - [Tipos de Schedulers](#tipos-schedulers)
  - [Mejores Prácticas de Schedulers](#mejores-practicas-schedulers)

- [Back Pressure / Overflow Strategy](#backpressure-overflow-strategy)
  - [Estrategias](#estrategias-backpressure)
  - [`onBackpressureBuffer()`, `onBackpressureDrop()`, etc.](#onbackpressurebuffer-drop)
  - [Concepto de Request Management](#request-management)

- [Batching / Windowing / Grouping](#batching-windowing-grouping)
  - [`buffer()`, `window()`, `groupBy()`](#buffer-window-groupby)

- [Repeat & Retry](#repeat-retry-reactive)
  - [`repeat()`](#repeat-reactive)
  - [`retry()`](#retry-reactive)
  - [`retryWhen()`](#retrywhen-reactive-advanced)

- [Sinks](#sinks-advanced)
  - [`Sinks.One`, `Sinks.Many`, `Sinks.Empty`](#sinks-one-many-empty-advanced)
  - [Concepto de `emit()` y su seguridad](#emit-safety)
  - [Uso para integrar APIs que no son reactivas](#integrar-apis-no-reactivas)

- [Context](#context)
  - [`ContextView` y `Context`](#contextview-context)
  - [`contextWrite()`](#contextwrite)
  - [`deferContextual()`](#defercontextual)
  - [Casos de uso](#casos-uso-context)

- [Pruebas unitarias con Step Verifier](#unit-testing-step-verifier)
  - [La importancia de las pruebas reactivas](#importancia-pruebas-reactivas)
  - [`StepVerifier`](#stepverifier)
    - [`create()`](#create-stepverifier)
    - [`expectNext()`, `expectNextCount()`, `expectNextSequence()`](#expectnext-stepverifier)
    - [`expectError()`, `expectComplete()`](#expecterror-expectcomplete-stepverifier)
    - [`expectSubscription()`](#expectsubscription-stepverifier)
    - [`thenRequest()`](#thenrequest-stepverifier)
    - [`verifyComplete()`, `verifyError()`](#verifycomplete-verifyerror-stepverifier)

<a id="introduction-reactive"></a>
## Introduction

<a id="antecedentes"></a>
### Antecedentes

<a id="problemas-imperativa-sincrona"></a>
#### Problemas de la programación imperativa/síncrona

<a id="problemas-asincrona-tradicional"></a>
#### Problemas de la programación asíncrona tradicional

<a id="que-es-programacion-reactiva"></a>
#### ¿Qué es la Programación Reactiva?

<a id="beneficios-programacion-reactiva"></a>
#### Beneficios clave

<a id="process-thread-cpu-ram-scheduler"></a>
### Process / Thread / CPU / RAM / Scheduler

<a id="hilos-procesos-reactive"></a>
#### Repaso de Hilos y Procesos

<a id="cpu-ram-reactive"></a>
#### CPU y RAM

<a id="scheduler-reactive"></a>
#### Scheduler del Sistema Operativo

<a id="concurrencia-paralelismo-reactive"></a>
#### Concurrencia vs. Paralelismo

<a id="efectos-secundarios-concurrencia-reactive"></a>
#### Efectos secundarios de la concurrencia

<a id="io-models"></a>
### IO Models

<a id="io-bloqueante"></a>
#### I/O Bloqueante (Blocking I/O)

<a id="io-no-bloqueante"></a>
#### I/O No Bloqueante (Non-Blocking I/O)

<a id="io-asincrono"></a>
#### I/O Asíncrono (Asynchronous I/O)

<a id="java-nio-nio2"></a>
#### Java NIO y NIO.2 (AIO)

<a id="communication-patterns"></a>
### Communication Patterns

<a id="sincrono-request-response"></a>
#### Síncrono Request/Response

<a id="asincrono-request-response"></a>
#### Asíncrono Request/Response

<a id="streaming-reactive"></a>
#### Streaming

<a id="event-driven-architectures"></a>
#### Event-Driven Architectures

<a id="what-is-reactive-programming"></a>
### What is Reactive Programming?

<a id="definicion-formal-reactive"></a>
#### Definición formal

<a id="orientada-eventos-vs-reactiva"></a>
#### Programación Orientada a Eventos vs. Reactiva

<a id="ventajas-enfoque-reactivo"></a>
#### Ventajas del enfoque reactivo

<a id="reactive-streams-specification"></a>
### Reactive Streams Specification

<a id="origenes-proposito-reactive-streams"></a>
#### Orígenes y propósito

<a id="interfaces-clave-reactive-streams"></a>
#### Interfaces Clave

<a id="reglas-garantias-reactive-streams"></a>
#### Reglas y Garantías de la Especificación

<a id="publisher-subscriber-communication"></a>
### Publisher/Subscriber Communication - Step By Step

<a id="demostracion-flujo-reactive"></a>
#### Demostración del flujo

<a id="rol-request-backpressure"></a>
#### El rol de `request()` (Backpressure)

<a id="diagramas-marble"></a>
#### Diagramas Marble (Marble Diagrams)

<a id="mono"></a>
## Mono

<a id="concepto-mono"></a>
### Concepto de `Mono`

<a id="emision-0-o-1-elemento"></a>
#### Emisión de 0 o 1 elemento

<a id="lazy-vs-eager-mono"></a>
#### Lazy vs. Eager

<a id="creacion-monos"></a>
### Creación de `Mono`s

<a id="mono-just-empty"></a>
#### `Mono.just()` y `Mono.empty()`

<a id="mono-error"></a>
#### `Mono.error()`

<a id="mono-fromcallable-runnable-supplier"></a>
#### `Mono.fromCallable()`, `Mono.fromRunnable()`, `Mono.fromSupplier()`

<a id="mono-defer"></a>
#### `Mono.defer()`

<a id="mono-fromfuture-completionstage"></a>
#### `Mono.fromFuture()`, `Mono.fromCompletionStage()`

<a id="transformacion-monos"></a>
### Transformación de `Mono`s

<a id="map-mono"></a>
#### `map()`

<a id="flatmap-mono"></a>
#### `flatMap()`

<a id="flatmapmany-mono"></a>
#### `flatMapMany()`

<a id="manejo-errores-mono"></a>
### Manejo de Errores en `Mono`

<a id="onerrorreturn-mono"></a>
#### `onErrorReturn()`

<a id="onerrorresume-mono"></a>
#### `onErrorResume()`

<a id="doonerror-mono"></a>
#### `doOnError()`

<a id="retry-mono"></a>
#### `retry()`

<a id="dofinally-doonterminate-mono"></a>
#### `doFinally()`, `doOnTerminate()`

<a id="manejo-eventos-ciclo-vida-mono"></a>
### Manejo de Eventos del Ciclo de Vida

<a id="operadores-condicion-filtrado-mono"></a>
### Operadores de Condición y Filtrado

<a id="combinacion-monos"></a>
### Combinación de `Mono`s

<a id="zipwith-mono"></a>
#### `zipWith()`

<a id="and-mono"></a>
#### `and()`

<a id="then-mono"></a>
#### `then()`

<a id="suscripcion-mono"></a>
### Suscripción a un `Mono`

<a id="subscribe-mono"></a>
#### `subscribe()`

<a id="block-mono"></a>
#### `block()`

<a id="tofuture-tocompletionstage-mono"></a>
#### `toFuture()`, `toCompletionStage()`

<a id="flux"></a>
## Flux

<a id="concepto-flux"></a>
### Concepto de `Flux`

<a id="emision-0-a-n-elementos"></a>
#### Emisión de 0 a N elementos

<a id="lazy-execution-flux"></a>
#### Lazy Execution

<a id="creacion-fluxs"></a>
### Creación de `Flux`s

<a id="flux-just"></a>
#### `Flux.just()`

<a id="flux-fromiterable-array-stream"></a>
#### `Flux.fromIterable()`, `Flux.fromArray()`, `Flux.fromStream()`

<a id="flux-range"></a>
#### `Flux.range()`

<a id="flux-interval"></a>
#### `Flux.interval()`

<a id="flux-generate"></a>
#### `Flux.generate()`

<a id="flux-create"></a>
#### `Flux.create()`

<a id="flux-error-empty"></a>
#### `Flux.error()`, `Flux.empty()`

<a id="transformacion-fluxs"></a>
### Transformación de `Flux`s

<a id="map-flux"></a>
#### `map()`

<a id="flatmap-flux"></a>
#### `flatMap()`

<a id="concatmap-flux"></a>
#### `concatMap()`

<a id="groupby-flux"></a>
#### `groupBy()`

<a id="filtrado-condicion-flux"></a>
### Filtrado y Condición

<a id="combinacion-fluxs"></a>
### Combinación de `Flux`s

<a id="merge-flux"></a>
#### `merge()`

<a id="concat-flux"></a>
#### `concat()`

<a id="zip-flux"></a>
#### `zip()`

<a id="combinelatest-flux"></a>
#### `combineLatest()`

<a id="withlatestfrom-flux"></a>
#### `withLatestFrom()`

<a id="agregacion-reduccion-flux"></a>
### Agregación y Reducción

<a id="reduce-flux"></a>
#### `reduce()`

<a id="collectlist-map-multimap-flux"></a>
#### `collectList()`, `collectMap()`, `collectMultimap()`

<a id="count-haselements-flux"></a>
#### `count()`, `hasElements()`

<a id="manejo-errores-flux"></a>
### Manejo de Errores en `Flux`

<a id="suscripcion-flux"></a>
### Suscripción a un `Flux`

<a id="subscribe-flux"></a>
#### `subscribe()`

<a id="blockfirst-blocklast-flux"></a>
#### `blockFirst()`, `blockLast()`

<a id="operators"></a>
## Operators

<a id="categorias-operadores"></a>
### Categorías de Operadores

<a id="profundizacion-operadores-comunes"></a>
### Profundización en Operadores Comunes

<a id="operadores-error-handling-reactive"></a>
### Operadores de Error Handling

<a id="retrywhen-reactive"></a>
#### `retryWhen()`

<a id="using-reactive"></a>
#### `using()`

<a id="backpressure-operadores"></a>
### Backpressure y Operadores

<a id="composicion-operadores"></a>
### Composición de Operadores

<a id="hot-cold-publishers"></a>
## Hot & Cold Publishers

<a id="cold-publishers"></a>
### Publishers Fríos (Cold Publishers)

<a id="definicion-cold-publishers"></a>
#### Definición

<a id="analogia-cold-publishers"></a>
#### Analogía

<a id="ejemplos-cold-publishers"></a>
#### Ejemplos

<a id="caracteristicas-cold-publishers"></a>
#### Características

<a id="hot-publishers"></a>
### Publishers Calientes (Hot Publishers)

<a id="definicion-hot-publishers"></a>
#### Definición

<a id="analogia-hot-publishers"></a>
#### Analogía

<a id="ejemplos-hot-publishers"></a>
#### Ejemplos

<a id="caracteristicas-hot-publishers"></a>
#### Características

<a id="transformar-cold-a-hot"></a>
### Transformar Cold a Hot

<a id="publish-autoconnect"></a>
#### `publish()` / `autoConnect()`

<a id="share-reactive"></a>
#### `share()`

<a id="cache-reactive"></a>
#### `cache()`

<a id="uso-sinks"></a>
### Uso de `Sinks`

<a id="sinks-one-many-empty"></a>
#### `Sinks.One`, `Sinks.Many`, `Sinks.Empty`

<a id="como-usar-sinks"></a>
#### Cómo usar `Sinks`

<a id="threading-schedulers"></a>
## Threading & Schedulers (Optional)

<a id="modelo-hilo-project-reactor"></a>
### El modelo de hilo de Project Reactor

<a id="subscribeon"></a>
#### `subscribeOn()`

<a id="publishon"></a>
#### `publishon()`

<a id="schedulers-project-reactor"></a>
### Schedulers de Project Reactor

<a id="concepto-schedulers"></a>
#### Concepto

<a id="tipos-schedulers"></a>
#### Tipos de Schedulers

<a id="mejores-practicas-schedulers"></a>
### Mejores Prácticas de Schedulers

<a id="backpressure-overflow-strategy"></a>
## Back Pressure / Overflow Strategy (Optional)

<a id="estrategias-backpressure"></a>
### Estrategias

<a id="onbackpressurebuffer-drop"></a>
#### `onBackpressureBuffer()`, `onBackpressureDrop()`, etc.

<a id="request-management"></a>
### Concepto de Request Management

<a id="batching-windowing-grouping"></a>
## Batching / Windowing / Grouping (Optional)

<a id="buffer-window-groupby"></a>
### `buffer()`, `window()`, `groupBy()`

<a id="repeat-retry-reactive"></a>
## Repeat & Retry (Optional)

<a id="repeat-reactive"></a>
### `repeat()`

<a id="retry-reactive"></a>
### `retry()`

<a id="retrywhen-reactive-advanced"></a>
### `retryWhen()`

<a id="sinks-advanced"></a>
## Sinks (Optional)

<a id="sinks-one-many-empty-advanced"></a>
### `Sinks.One`, `Sinks.Many`, `Sinks.Empty`

<a id="emit-safety"></a>
### Concepto de `emit()` y su seguridad

<a id="integrar-apis-no-reactivas"></a>
### Uso para integrar APIs que no son reactivas

<a id="context"></a>
## Context (Optional)

<a id="contextview-context"></a>
### `ContextView` y `Context`

<a id="contextwrite"></a>
### `contextWrite()`

<a id="defercontextual"></a>
### `deferContextual()`

<a id="casos-uso-context"></a>
### Casos de uso

<a id="unit-testing-step-verifier"></a>## Unit Testing With Step Verifier (Optional)

<a id="importancia-pruebas-reactivas"></a>### La importancia de las pruebas reactivas

<a id="stepverifier"></a>
### `StepVerifier`

<a id="create-stepverifier"></a>
#### `create()`

<a id="expectnext-stepverifier"></a>
#### `expectNext()`, `expectNextCount()`, `expectNextSequence()`

<a id="expecterror-expectcomplete-stepverifier"></a>
#### `expectError()`, `expectComplete()`

<a id="expectsubscription-stepverifier"></a>
#### `expectSubscription()`

<a id="thenrequest-stepverifier"></a>
#### `thenRequest()`

<a id="verifycomplete-verifyerror-stepverifier"></a>
#### `verifyComplete()`, `verifyError()`

