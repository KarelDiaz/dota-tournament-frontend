<template>
  <div>
    <form @submit.prevent="send">
      <!-- Header controls -->
      <div class="flex justify-center pb-3 space-x-3">
        <!-- Good -->
        <label
          :class="[
            'w-full text-center border bg-gradient-to-b rounded-lg shadow-lg',
            {
              'cursor-pointer text-gray-900 border-gray-300 hover:border-gray-200 from-gray-100 to-gray-300 hover:from-gray-50 hover:to-gray-200':
                playForm.side_win === 'bad',
            },
            {
              'cursor-default text-yellow-900 border-yellow-400 from-yellow-200 to-yellow-400':
                playForm.side_win === 'good',
            },
          ]"
        >
          <input
            v-model="playForm.side_win"
            type="radio"
            name="radio"
            value="good"
            hidden
          />
          Good
        </label>
        <!-- Save -->
        <button
          class="px-3 text-blue-900 border border-blue-400 rounded-lg shadow-lg sm:w-40 hover:border-blue-300 bg-gradient-to-b from-blue-200 to-blue-400 hover:from-blue-100 hover:to-blue-300"
          type="submit"
        >
          Save
        </button>
        <!-- Bad -->
        <label
          :class="[
            'rounded-lg shadow-lg w-full text-center border bg-gradient-to-b',
            {
              'cursor-pointer text-gray-900 border-gray-300 hover:border-gray-200 from-gray-100 to-gray-300 hover:from-gray-50 hover:to-gray-200':
                playForm.side_win === 'good',
            },
            {
              'cursor-default text-yellow-900 border-yellow-400 from-yellow-200 to-yellow-400':
                playForm.side_win === 'bad',
            },
          ]"
        >
          <input
            v-model="playForm.side_win"
            type="radio"
            name="radio"
            value="bad"
            hidden
          />
          Bad
        </label>
      </div>
      <!-- Players -->
      <div
        class="flex flex-wrap overflow-hidden border border-t-0 border-b-0 rounded-lg shadow-lg "
      >
        <!-- Player list -->
        <div
          class="flex flex-col w-1/5 bg-white sm:w-1/10"
          v-for="(pr, index) in playForm.player_results"
          :key="index"
        >
          <!-- Player or Bot button -->
          <label
            :class="[
              'cursor-pointer bg-gradient-to-bl border text-center border-l-0 border-r-0',
              {
                'border-gray-300 hover:border-gray-200 from-gray-100 to-gray-300 hover:from-gray-50 hover:to-gray-200':
                  pr.bot,
              },
              {
                'border-blue-300 hover:border-blue-200 from-blue-100 to-blue-300 hover:from-blue-50 hover:to-blue-200':
                  !pr.bot,
              },
            ]"
          >
            <span v-if="pr.bot">Bot</span>
            <span v-if="!pr.bot">Player</span>
            <input v-model="pr.bot" type="checkbox" hidden />
          </label>
          <!-- Side -->
          <div class="flex">
            <div
              v-if="pr.side === 'good'"
              class="w-full h-2 bg-gradient-to-b from-green-200 to-green-400"
            ></div>
            <div
              v-if="pr.side === 'bad'"
              class="w-full h-2 bg-gradient-to-b from-red-200 to-red-400"
            ></div>
          </div>
          <!-- Player form -->
          <div
            :class="[
              'bg-opacity-10 flex flex-col justify-between h-40 p-1 bg-center bg-cover shadow-inner',
              { 'bg-green-400': pr.side === 'good' },
              { 'bg-red-400': pr.side === 'bad' },
            ]"
            :style="[
              {
                'background-image':
                  'url(' +
                  $store.state.local +
                  (pr.bot
                    ? '/npc/bot.png'
                    : `/npc/${getHero(pr.hero).name}.png`) +
                  ')',
              },
            ]"
          >
            <!-- Player -->
            <select
              class="w-full py-1 bg-opacity-50 border border-gray-200 border-opacity-50 bg-gray-50"
              required
              v-model="pr.player"
              v-if="!pr.bot"
            >
              <option :value="p.id" v-for="p in players" :key="p.id">
                {{ p.nick }}
              </option>
            </select>
            <!-- KDA -->
            <div class="flex" v-if="!pr.bot">
              <!-- K -->
              <div
                class="flex flex-col text-center bg-opacity-50 border border-r-0 border-gray-200 border-opacity-50 bg-gray-50"
              >
                <span class="text-xs">K</span>
                <input
                  class="w-full text-center bg-white bg-opacity-0"
                  v-model="pr.kills"
                  v-if="!pr.bot"
                  type="text"
                  placeholder="Kills"
                />
              </div>
              <!-- D -->
              <div
                class="flex flex-col text-center bg-opacity-50 border border-r-0 border-gray-200 border-opacity-50 bg-gray-50"
              >
                <span class="text-xs">D</span>
                <input
                  class="w-full text-center bg-white bg-opacity-0"
                  v-model="pr.deths"
                  v-if="!pr.bot"
                  type="text"
                  placeholder="Deths"
                />
              </div>
              <!-- A -->
              <div
                class="flex flex-col text-center bg-opacity-50 border border-gray-200 border-opacity-50 bg-gray-50"
              >
                <span class="text-xs">A</span>
                <input
                  class="w-full text-center bg-white bg-opacity-0"
                  v-model="pr.asist"
                  v-if="!pr.bot"
                  type="text"
                  placeholder="Asist"
                />
              </div>
            </div>
            <!-- Hero -->
            <select
              class="w-full py-1 bg-opacity-50 border border-gray-200 border-opacity-50 bg-gray-50"
              v-if="!pr.bot"
              v-model="pr.hero"
              required
            >
              <option :value="h.id" v-for="h in heroes" :key="h.id">
                {{ h.displayName }}
              </option>
            </select>
          </div>
          <!-- win -->
          <div class="flex">
            <div
              v-if="pr.win"
              class="w-full h-2 bg-gradient-to-b from-yellow-200 to-yellow-400"
            ></div>
            <div
              v-if="!pr.win"
              class="w-full h-2 bg-gradient-to-b from-gray-200 to-gray-400"
            ></div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import { mapState, mapMutations, mapGetters } from "vuex";

import { INIT_PLAYERS } from "@/store/type/mutations";
import { GET_PLAYER, GET_HERO } from "@/store/type/getters";

import MMR from "@/store/model/mmr";
import Player from "@/store/model/player";
import Play from "@/store/model/play";

export default {
  data() {
    return {
      plays: [],
      playForm: Play,
    };
  },
  props: {
    teamGood: {},
    teamBad: {},
  },
  computed: {
    ...mapGetters([GET_PLAYER, GET_HERO]),
    ...mapState({
      strapi: (state) => state.strapi,
      players: (state) => state.players,
      heroes: (state) => state.heroes,
    }),
  },
  methods: {
    ...mapMutations([INIT_PLAYERS]),

    send() {
      var out = [];

      // players to win or lose
      var playersWin = [];
      var mediaWin = 0;
      var playersLose = [];
      var mediaLose = 0;
      this.playForm.player_results.forEach((pr) => {
        pr.win = this.playForm.side_win == pr.side;

        if (!pr.bot) {
          if (pr.win) {
            playersWin.push(pr.player);
            mediaWin += this.getPlayer(pr.player).mmr;
          }
          if (!pr.win) {
            playersLose.push(pr.player);
            mediaLose += this.getPlayer(pr.player).mmr;
          }
        }
      });

      if (playersWin.length > 0 && playersLose.length > 0) {
        this.mmr(playersWin, playersLose);
        mediaWin /= playersWin.length;
        mediaLose /= playersLose.length;
      }

      this.playForm.player_results.forEach((pr) => {
        if (!pr.bot) {
          pr.mmr = this.getPlayer(pr.player).mmr;
          if (pr.win) {
            let tmmr = new MMR(pr.mmr, mediaLose);
            pr.mmrPlus = tmmr.getPlusA();
          } else {
            let tmmr = new MMR(mediaWin, pr.mmr);
            pr.mmrPlus = tmmr.getPlusB();
          }
        }
        axios.post(this.strapi + "/player-results", pr).then(({ data }) => {
          out.push(data.id);

          if (out.length === 10) {
            axios
              .post(
                this.strapi + "/plays",
                new Play(out, this.playForm.side_win)
              )
              .then(({ data }) => {
                this.$emit("added", data);
                this.resetForm();
                this.initPlayers();
              });
          }
        });
      });
    },

    mmr(win, lose) {
      var pWin = [];
      var pLose = [];

      // media del ELO q ganan
      let mmrWin = 0;
      win.forEach((id) => {
        let ptemp = this.getPlayer(id);
        mmrWin += ptemp.mmr;
        pWin.push(ptemp);
      });
      mmrWin /= win.length;

      // media del ELO q pierden
      let mmrLose = 0;
      lose.forEach((id) => {
        let ptemp = this.getPlayer(id);
        mmrLose += ptemp.mmr;
        pLose.push(ptemp);
      });
      mmrLose /= lose.length;

      // update mmr de los q ganana
      pWin.forEach((player) => {
        let mmr = new MMR(player.mmr, mmrLose);
        let playerOut = new Player(
          player.fullName,
          player.nick,
          mmr.getMMRA(),
          player.active
        );

        axios
          .put(`${this.strapi}/players/${player.id}`, playerOut)
          .then(() => {});
      });

      // update mmr de los q pierden
      pLose.forEach((player) => {
        let mmr = new MMR(mmrWin, player.mmr);
        let playerOut = new Player(
          player.fullName,
          player.nick,
          mmr.getMMRB(),
          player.active
        );

        axios
          .put(`${this.strapi}/players/${player.id}`, playerOut)
          .then(() => {});
      });
    },

    resetForm() {
      let side_win = this.playForm.side_win;
      this.playForm = new Play([], side_win);
      // in case of trounament the play should have the players from scratch
      // good side
      if (this.teamGood)
        this.teamGood.players.forEach((player) => {
          let prtemp = this.playForm.player_results.find(
            (pr) => pr.side == "good" && pr.bot
          );
          prtemp.player = player;
          prtemp.bot = false;
        });
      // bad side
      if (this.teamBad)
        this.teamBad.players.forEach((player) => {
          let prtemp = this.playForm.player_results.find(
            (pr) => pr.side == "bad" && pr.bot
          );
          prtemp.player = player;
          prtemp.bot = false;
        });

      this.playForm.player_results.sort((a, b) => {
        if (a.side == b.side)
          if (a.side === "good") return a.bot ? -1 : 1;
          else return a.bot ? 1 : -1;

        return b.side.localeCompare(a.side);
      });
    },
  },
  watch: {
    "playForm.side_win"() {
      this.playForm.player_results.forEach((pr) => {
        pr.win = this.playForm.side_win === pr.side;
      });
    },
    teamGood() {
      this.resetForm();
    },
    teamBad() {
      this.resetForm();
    },
  },
  created() {
    this.resetForm();
  },
};
</script>