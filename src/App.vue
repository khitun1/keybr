<script setup lang="ts">
  import {ref} from "vue";

  const letters = ['A', 'B', 'C', 'D', 'E', 'F',
                    'G', 'H', 'I', 'J', 'K',
                    'L', 'M', 'N', 'O', 'P',
                    'Q', 'R','S', 'T', 'U',
                    'V', 'W', 'X', 'Y', 'Z'];

  const keyboard = ['`', '1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '-', '=', 'Backspace',
                    'Tab', 'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P', '[', ']', '\\',
                    'Caps Lock', 'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', ';', '\'', 'Enter',
                    'Shift', 'Z', 'X', 'C', 'V', 'B', 'N', 'M', ',', '.', '/', 'Shift',
                    'Ctrl', 'Alt', 'Space'];

  const actives = ref<string[]>([]);

  const text = ref<string>('');

  const textArr = ref<string[]>([]);

  const count = ref(0);

  const errLaw = ref(false);

  const addActive = (letter: string) => {
    const cur = document.getElementById('sel' + letter)?.classList;
    if (cur.contains('active')) {
      cur.remove('active');
      actives.value.splice(actives.value.indexOf(letter),1);
    }
    else {
      cur.add('active');
      actives.value.push(letter);
    }
    generateText();
  }
  const generateString = () => {
    let str = '';
    const length = generateStrLength();
    for (let i = 0; i < length; i++) {
      str += actives.value[Math.floor(Math.random() * actives.value.length)];
    }
    return str;
  }

  const generateStrLength = () => {
    return Math.floor(Math.random() * 4 + 3);
  }

  const generateText = () => {
    if (actives.value.length !== 0) {
      for (let i = 0; i < text.value.length; i++) {
        document.getElementById(i + textArr.value[i]).classList.remove('red');
        document.getElementById(i + textArr.value[i]).classList.remove('gray');
      }

      text.value = '';
      for (let i = 0; i < 20; i++) {
        text.value += generateString();
        text.value += '*';
      }

      textArr.value = [...text.value];
      document.getElementById('generate').blur();
      errLaw.value = false;
    }
  }

  document.addEventListener("keydown", (e) => {
    const upper = e.key.toUpperCase();
    let color = '';
    document.getElementById(upper)?.classList.add('active');
    if (textArr.value[count.value] === upper) {
      color = errLaw.value ? 'red' : 'gray';
      console.log(color)
      document.getElementById(count.value + upper)?.classList.add(color);
      count.value++;
      errLaw.value = false;
    }
    else if (textArr.value[count.value] === '*' && upper === ' ') {
      color = errLaw.value ? 'red' : 'gray';
      document.getElementById(count.value + '*')?.classList.add(color)
      count.value++;
      errLaw.value = false;
    }
    else {
      errLaw.value = true;
    }
    if (count.value === text.value.length) {
      generateText();
    }
  }
  );

  document.addEventListener("keyup", (e) => {
    document.getElementById(e.key.toUpperCase())?.classList.remove('active');
  });

</script>

<template>
<div id="main">
  <span id="choice">
    <h3>Select the letters</h3>
    <button v-for="letter in letters" :key="letters.indexOf(letter)" @click="addActive(letter)"
            :id="'sel'+letter" class=":active">
        {{letter}}
    </button>
  </span>
  <div id="text">
    <p v-for="(char, index) in textArr" :key="index" :id="index + char">
      {{char}}
    </p>
  </div>
  <div id="keyboard">
    <p v-for="(item, index) in keyboard" :key="index" :id="item">
      {{item}}
    </p>
  </div>
</div>
</template>

<style scoped lang="scss">

#main {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;

}

  #keyboard {
    display: flex;
    background: #dcdcdc;
    width: 850px;
    flex-wrap: wrap;
    border-radius: 10px;
    font-size: 24px;
    margin-top: 50px;

    p {
      background: #adadad;
      width: fit-content;
      padding: 15px;
      margin: 5px 5px 5px 5px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 5px;
    }
    .active {
      background: #6c6c6c;
    }
    #Space {
      width: 450px;
      color: #adadad;
    }
    #Shift {
      width: 110px;
    }
  }

#choice {
  .active {
    background: #8c8c8c;
  }

  margin-top: 50px;
  button {
    margin-right: 5px;
    cursor: pointer;
  }
}

#text {
  background: #dadada;
  margin-top: 50px;
  width: 50%;
  padding: 10px;
  height: fit-content;
  display: flex;
  flex-wrap: wrap;
  p {
    font-size: 26px;
    margin: 5px 0;
  }
  .gray{
    color: #868686;
  }
  .red{
    color: #ff2929;
  }
}
</style>
