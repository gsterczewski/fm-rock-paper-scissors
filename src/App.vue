<template>
<div class="layout-wrapper">
<RulesModal :isActive="isRulesModalActive" :handleClose="hideRulesModal" />
<VHeader>
  <ScoreBoard :score="score" />
</VHeader>
<main>
    <GameBoard :variants="Object.values(possibleFigures)"
      :handleFigureSelect="selectPlayerFigure" 
      :gameStage="currentGameStage" 
      :handlePlayAgain="playAgain"
      :playerFigure="possibleFigures[playerFigure]"
      :houseFigure="possibleFigures[houseFigure]"
     />
</main>
  <div class="rules-button-container">
    <VButton name="Rules" @click="showRulesModal"  />
  </div>
</div>
</template>

<script>
import VHeader from "./components/VHeader.vue";
import ScoreBoard from "./components/ScoreBoard.vue";
import GameBoard from "./components/GameBoard.vue";
import VButton from "./components/VButton.vue";
import RulesModal from "./components/RulesModal.vue";
import { onMounted, watch } from "vue";
import useLocalStore from "./hooks/useLocalStore";
import useGameLogic from "./hooks/useGameLogic";
import useModal from "./hooks/useModal";

export default {
  components: { VHeader, VButton, GameBoard, ScoreBoard, RulesModal },
  setup(){
    
    const gameStages = {
      beforeSelect : 0,
      selected : 1,
      won : 2,
      lost : 3,
      draw : 4 
    };
    const possibleFigures = {
      0 : "rock",
      1 : "paper",
      2 : "scissors",
      3 : "lizard",
      4 : "spook"
    };
    
    const { isRulesModalActive, showRulesModal, hideRulesModal } = useModal();
    const { loadScore, saveScore } = useLocalStore();
    const { score, currentGameStage, playerFigure, houseFigure, playAgain, selectPlayerFigure } = useGameLogic(possibleFigures, gameStages);

    onMounted(() => loadScore(score));
    watch(score, ()=> saveScore(score.value));

    return {
      score,
      currentGameStage,
      playerFigure,
      houseFigure,
      possibleFigures,
      isRulesModalActive,
      showRulesModal,
      hideRulesModal,
      selectPlayerFigure,
      playAgain
    };
  }
};

</script>

<style lang="scss">

*{
  box-sizing: border-box;
  margin:0;
}
body{
  font-family: 'Barlow Semi Condensed', sans-serif;
}

.layout-wrapper{
  background-image: var(--gradient-background);
  background-size: cover;
  background-repeat: no-repeat;
  padding: 2rem 1rem 1rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;

  @media(min-height:1000px){
    justify-content: space-around;
  }
}


main{
  width:100%;
  max-width:1000px;
  min-width:320px;
  height: 60vh;
  display: flex;
  align-items: center;
}

.rules-button-container {
  margin-bottom: 4rem;
}
@media(min-width:$desktop-breakpoint){
  .rules-button-container{
    width:100%;
    display: flex;
    justify-content: flex-end;
    padding:1rem;
    margin-bottom:0;
  }
  main {
    margin-bottom:-4rem;

  }
}
</style>
