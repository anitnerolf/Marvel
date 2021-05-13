<template>
  <div class="marvel">
    <h1> {{ msg }} </h1>
        <div class="container">
        <form class="search-characters" v-on:submit.prevent="getComic">
          <input
            type="text"
            class="form-control text-muted form-rounded p-4 shadow-sm"
            placeholder="Search for a character"
            v-model="character"
            autocomplete="off"
          />
      </form>
      </div>
  </div>
</template>

<script>
var md5 = require('md5')
// import md5 from 'js-md5';
export default {
  name: 'Marvel',
  data () {
    return {
      character: '',
      loading: false,
      comics: [],
      reversed: [],
      showcomic: [],
      collection: {
        public_key: '232de3cd47562ba3dce8a33d7118ab13',
        private_key: 'a6caf73e1c2ee99586459bef36ae464e02efd6d4'
      }
    }
  },
  computed: {
    hashed () {
      let concat = this.unixtime + this.collection.public_key + this.collection.private_key
      return md5(concat)
    },
    unixtime () {
      let datenow = Date.now()
      var ts = Math.floor(datenow / 1000)
      return ts
    }
  },
  methods: {
    getComic () {
      this.loading = true
      window.axios.get('https://gateway.marvel.com/v1/public/comics', {
        params: {
          apikey: this.collection.public_key,
          hash: this.hashed,
          dateDescriptor: 'thisWeek'
        }
      })
        .then(response => {
          this.loading = false
          this.reversed = response.data.data.results
          this.comics = this.reversed.reverse()
        }).catch(e => {
          console.log(e)
        })
    },
  },
  // data () {
  //   return {
  //     character: "",
  //   };
  // },
  // methods: {
  //   getCharacters: async function () {
  //     console.log("--->", this.character);
  //     const PUBLIC_KEY = '4b4d1472a8187a096ac07fb167870a21';
  //     const PRIVATE_KEY = '70963cc72718ee87f8311ad5c4e913fca8e3a91f';
  //     // const marvelPublic = '4b4d1472a8187a096ac07fb167870a21';
  //     // const marvelPrivate = '70963cc72718ee87f8311ad5c4e913fca8e3a91f';
  //     const ts = Number(new Date());
  //     // const marvelURL = ''
  //     const marvelURL= `https://gateway.marvel.com/v1/public/characters?ts=${ts}&apikey=${PUBLIC_KEY}&hash=${PRIVATE_KEY}`;
  //     // const marvelURL = 'https://gateway.marvel.com/v1/public/characters?ts=1&apikey=${marvelPublic}&hash=${marvelPrivate}&name=${this.character}';
  //     try {
  //         const response = await fetch(marvelURL);
  //         const data = await response.json();
  //         this.character = "";
  //         console.log("-->", data);
  //       } catch (error) {

  //         console.log(error);
  //       }
  //   }
  // },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
</style>
