# Используйте Async-Await или обещания для асинхронной обработки ошибок

### Объяснение в один абзац

Обратные вызовы плохо масштабируются, так как большинство программистов не знакомы с ними. Они заставляют проверять ошибки повсюду, имеют дело с неприятным вложением кода и затрудняют анализ потока кода. Библиотеки Promise, такие как BlueBird, async и Q, упаковывают стандартный стиль кода, используя RETURN и THROW для управления потоком программы. В частности, они поддерживают любимый стиль обработки ошибок try-catch, который позволяет освободить путь к основному коду от ошибок в каждой функции

### Пример кода – использование обещаний для отлова ошибок

```javascript
return functionA()
  .then(functionB)
  .then(functionC)
  .then(functionD)
  .catch((err) => logger.error(err))
  .then(alwaysExecuteThisFunction)
```


### Пример кода - использование async/await для отлова ошибок

```javascript
async function executeAsyncTask () {
  try {
    const valueA = await functionA();
    const valueB = await functionB(valueA);
    const valueC = await functionC(valueB);
    return await functionD(valueC);
  }
  catch (err) {
    logger.error(err);
  } finally {
    await alwaysExecuteThisFunction();
  }
}
```

### Антипаттерн. Обработка ошибок в стиле обратного вызова

<details>
<summary><strong>Javascript</strong></summary>

```javascript
getData(someParameter, function(err, result) {
    if(err !== null) {
        // do something like calling the given callback function and pass the error
        getMoreData(a, function(err, result) {
            if(err !== null) {
                // do something like calling the given callback function and pass the error
                getMoreData(b, function(c) {
                    getMoreData(d, function(e) {
                        if(err !== null ) {
                            // you get the idea?
                        }
                    })
                });
            }
        });
    }
});
```
</details>

<details>
<summary><strong>Typescript</strong></summary>

```typescript
getData(someParameter, function(err: Error | null, resultA: ResultA) {
  if(err !== null) {
    // do something like calling the given callback function and pass the error
    getMoreData(resultA, function(err: Error | null, resultB: ResultB) {
      if(err !== null) {
        // do something like calling the given callback function and pass the error
        getMoreData(resultB, function(resultC: ResultC) {
          getMoreData(resultC, function(err: Error | null, d: ResultD) {
            if(err !== null) {
              // you get the idea?
            }
          })
        });
      }
    });
  }
});
```
</details>

### Цитата из блога: "У нас проблема с обещаниями"

Из блога pouchdb.com

> … На самом деле, обратные вызовы делают что-то еще более зловещее: они лишают нас стека, что мы обычно принимаем как должное в языках программирования. Написание кода без стека очень похоже на вождение автомобиля без педали тормоза: вы не понимаете, насколько сильно оно вам нужно, пока не дойдете до него, а его там нет. Весь смысл обещаний состоит в том, чтобы вернуть нам языковые основы, которые мы потеряли при асинхронности: возврат, выброс и стек. Но вы должны знать, как правильно использовать обещания, чтобы воспользоваться ими.

### Цитата блога: "Метод обещаний гораздо более компактен"

Из блога gosquared.com

> … Метод обещаний гораздо компактнее, понятнее и быстрее для написания. Если ошибка или исключение происходят в любом из операций, они обрабатываются одним обработчиком .catch(). Наличие единого места для обработки всех ошибок означает, что вам не нужно писать проверку ошибок для каждого этапа работы.

### Цитата блога: "Обещания являются родными ES6, могут использоваться с генераторами"

Из блога StrongLoop

> … Обратные вызовы имеют паршивую историю обработки ошибок. Обещания лучше. Объедините встроенную обработку ошибок в Express с обещаниями и значительно снизьте вероятность возникновения необработанного исключения. Обещания являются родными ES6, могут использоваться с генераторами, а предложения ES7, такие как async/await, через компиляторы, такие как Babel.

### Цитата из блога: "Все те обычные конструкции управления потоком, к которым вы привыкли, полностью разрушены"

Из блога Бенно

> … Одно из лучших преимуществ асинхронного программирования на основе обратного вызова состоит в том, что в основном все эти обычные конструкции управления потоком, к которым вы привыкли, полностью разрушены. Тем не менее, я считаю, что больше всего разрушения коснулись обработки исключений. Javascript предоставляет довольно знакомую конструкцию try…catch для работы с исключениями. Проблема с исключениями состоит в том, что они обеспечивают отличный способ сокращения ошибок в стеке вызовов, но в конечном итоге оказываются совершенно бесполезными, если ошибка происходит в другом стеке…
