<script>
export default {
  data() {
    return {
      input: '',
      stdout: [],
      currentLocation: '~',
      fileItems: [
        'aboutme',
        'foss',
        'projects',
      ],
      commandItems: [
        'help',
        'clear'
      ],
      availableCommands: [
        'help',
        'clear',
        'cat',
        'ls',
        'cd',
        'date',
        'echo',
      ],
      aboutme: {
        location: 'Murcia - Spain',
        role: 'Software Developer',
        company: 'Datalife (2020)',
        interests: ['python', 'linux', 'web development', 'open source']
      },
      foss: [
        {
          name: 'Tryton',
          description: '',
          url: 'https://www.tryton.org',
          year: 2020
        },
      ],
      projects: [
          {
            name: 'Tryton Module Index',
            description: 'A web application to search and browse community Tryton modules.',
            url: '',
            year: 2025
          },
        ]
    }
  },
  methods: {
    focusInput() {
      const inputElement = document.querySelector('input')
      setTimeout(() => inputElement.focus(), 100)
    },
    emulateInput(value) {
      this.input = value
      this.doInput(value)
      this.focusInput()
    },
    doInput() {
      this.manageInput(this.input)
      this.input = ''
    },
    doAutocomplete() {
      const input = this.input
      if (!input) {
        return
      }
      const command = input.split(' ')[0]
      const args = input.split(' ').slice(1)
      if (args.length == 0) {
        const suggestions = this.availableCommands.filter(item => item.startsWith(command))
        if (suggestions.length === 1) {
          this.input = suggestions[0]
        }
        else if (suggestions.length > 1) {
          this.stdout.push({prompt: {loc: this.currentLocation, text: input}})
          this.stdout.push({text: suggestions.join('   '), class: 'text-blue-500'})
        }
      } else {
        switch (command) {
          case 'cat':
            const file = args[0]
            const suggestions = this.fileItems.filter(item => item.startsWith(file))
            if (suggestions.length === 1) {
              this.input = `cat ${suggestions[0]}.md`
            }
            else if (suggestions.length > 1) {
              this.stdout.push({prompt: {loc: this.currentLocation, text: input}})
              this.stdout.push({text: suggestions.join('   '), class: 'text-blue-500'})
            }
            break
          default:
            break
        }
      }
    },
    manageInput(value) {
      if (!value) {
        this.stdout.push({prompt: {loc: this.currentLocation, text: ''}})
        return
      }
      const command = value.split(' ')[0]
      const args = value.split(' ').slice(1)
      const stdout = document.getElementById('stdout')
      this.stdout.push({prompt: {loc: this.currentLocation, text: value}})
      switch (command) {
        case 'help':
          this.stdout.push({text: 'Available commands:', class: 'font-bold'})
          this.availableCommands.forEach(item => {
            this.stdout.push({text: item, class: 'ps-6'})
          })
          break
        case 'cat':
          const file = args[0]
          switch (file) {
            case 'aboutme.md':
              this.stdout.push({text: 'About me:', class: 'font-bold'})
              this.stdout.push({text: `Location: ${this.aboutme.location}`, class: 'ps-6'})
              this.stdout.push({text: `Company: ${this.aboutme.company}`, class: 'ps-6'})
              this.stdout.push({text: `Role: ${this.aboutme.role}`, class: 'ps-6'})
              this.stdout.push({text: `Interests: ${this.aboutme.interests.join(', ')}`, class: 'ps-6'})
              break
            case 'foss.md':
              this.stdout.push({text: 'Contributions for Free and Open Source Software:', class: 'font-bold'})
              this.foss.forEach(item => {
                this.stdout.push({text: `${item.name} (${item.year})`, class: 'ps-6 text-blue-500'})
                this.stdout.push({text: `${item.description}`, class: 'ps-6'})
                this.stdout.push({text: `URL: ${item.url}`, class: 'ps-6'})
              })
              break
            case 'projects.md':
              this.stdout.push({text: 'Personal projects:', class: 'font-bold'})
              this.projects.forEach(item => {
                this.stdout.push({text: `${item.name} (${item.year})`, class: 'ps-6 text-blue-500'})
                this.stdout.push({text: `${item.description}`, class: 'ps-6'})
                this.stdout.push({text: `URL: ${item.url}`, class: 'ps-6'})
              })
              break
            default:
              this.stdout.push({text: `No such file or directory: ${file}`, class: 'text-red-500'})
              break
          }
          break
        case 'clear':
          this.stdout = []
          break
        case 'date':
          this.dateOutput()
          break
        case 'echo':
          this.stdout.push({text: args.join(' ')})
          break
        case 'ls':
          let files = ['.']
          files.push(...this.fileItems.map(item => item + '.md'))
          const filesString = files.join('   ')
          this.stdout.push({text: filesString, class: 'text-blue-500'})
          break
        case 'cd':
          if (!args|| !args.length || args[0] === '.' || args[0] === './') {
            this.currentLocation = '~'
            return
          } else {
            this.stdout.push({text: `No such file or directory: ${args[0]}`, class: 'text-red-500'})
          }
          break
        default:
          this.stdout.push({text: `Command not found: ${command}`, class: 'text-red-500'})
          break
      }
      setTimeout(() => stdout.scrollTop = stdout.scrollHeight, 25)
    },
    dateOutput() {
      const now = new Date()
      this.stdout.push({text: `${now.toDateString()} ${now.toLocaleTimeString()}`, class: 'text-sky-500'})
    },
  },
  created() {
    this.dateOutput()
    this.stdout.push({text: 'Welcome to my personal website!'})
    this.stdout.push({text: 'Type `help` to see the available commands.'})
  },
  mounted() {
  }
}
</script>

<template>
  <div id="main" class="max-w-5xl mx-auto p-5 flex flex-col items-center text-center gap-2 h-full">
    <img class="logo border-2 border-white"
    src="https://www.gravatar.com/avatar/38215981cb9c9e28db73695b1f5d87b60c921f19c82a175bcf7cafe1a0c213d7?size=200"
    alt="avatar" />
    <h1 class="text-3xl mt-4 font-bold">@tiyujopite</h1>
    <h1 class="text-2xl font-semibold">José Antonio Díaz Miralles</h1>
    <div class="flex gap-4">
      <template v-for="item in fileItems">
        <button @click="emulateInput(`cat ${item}.md`)">+{{item}}</button>
      </template>
      <template v-for="item in commandItems">
        <button @click="emulateInput(item)">+{{item}}</button>
      </template>
    </div>
    <div id="terminal" class="max-w-5xl m-2 p-4 rounded-xl bg-gray-900 w-full h-full border border-neutral-700"
    @click="focusInput">
      <div id="stdout" class="flex flex-col gap-1 text-left  whitespace-pre">
        <template v-for="line in stdout">
          <div class="flex gap-2">
            <span v-if="line.prompt" class="text-green-400 font-semibold">{{ line.prompt.loc }}</span>
            <span v-if="line.prompt" class="text-green-400 font-semibold">$</span>
            <span v-if="line.prompt">{{ line.prompt.text }}</span>
            <div :class="line.class">{{ line.text }}</div>
          </div>
        </template>
      </div>
      <div class="flex gap-2 h-10 items-center">
        <span class="text-green-400 font-semibold">{{currentLocation}}</span>
        <span class="text-green-400 font-semibold">$</span>
        <input type="text" class="w-full bg-transparent border-none focus:outline-none m-0 p-0"
        autocomplete="off"
        autocapitalize="off"
        spellcheck="false"
        v-model="input"
        autofocus
        @keydown.tab.prevent="doAutocomplete"
        @keypress.enter="doInput"/>
      </div>
    </div>
  </div>
</template>

<style scoped>
#main {
  height: 100vh;
}
.logo {
  will-change: filter;
  transition: filter 300ms;
  border-radius: 50%;
}
.logo:hover {
  filter: drop-shadow(0 0 1em #48ff0085);
}

#terminal {
  background-color: #1e1e1e;
  height: max-content;
  max-height: max-content;
}

#stdout {
  max-height: 60vh;
  overflow-y: auto;
}

#stdout::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: .5em;
  background-color: #242424;
}

#stdout::-webkit-scrollbar {
  width: .5em;
  border-radius: .5em;
  background-color: #242424;
}

#stdout::-webkit-scrollbar-thumb {
  border-radius: .5em;
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
  background-color: white;
}
button:hover {
  cursor: pointer;
  text-decoration: underline;
}
</style>
