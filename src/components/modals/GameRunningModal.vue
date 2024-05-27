<template>
    <div id="gameRunningModal" :class="['modal', {'is-active': isOpen}]">
        <div class="modal-background" @click="close"></div>
        <div class="modal-content">
            <div class='notification is-info'>
                <h3 class="title" v-if="isRawMode">{{ activeGame.displayName }} is launching via EXE file</h3>
                <h3 class="title" v-else="isRawMode">{{ activeGame.displayName }} is starting using platform: {{ activeGame.activePlatform.storePlatform }}</h3>
                <h5 class="title is-5">Close this message to continue modding.</h5>
                <div v-if="isRawMode">
                    <p>Path to the game exe file: {{ gameDirectory }}\{{ activeGame.exeName[0] }}</p>
                </div>
                <div v-else="isRawMode">
                    <p>If this is taking a while, it's likely due to Steam starting.</p>
                    <p>Please be patient, and have fun!</p>
                </div>
            </div>
        </div>
        <button class="modal-close is-large" aria-label="close" @click="close"></button>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

import Game from "../../model/game/Game";
import ManagerSettings from "src/r2mm/manager/ManagerSettings";

@Component
export default class GameRunningModal extends Vue {
    @Prop({required: true})
    readonly activeGame!: Game;
    get settings(): ManagerSettings {
        return this.$store.getters['settings'];
    }

    get isRawMode(): boolean {
        return this.settings.getContext().gameSpecific.runRaw;
    }

    get gameDirectory(): string {
        return this.settings.getContext().gameSpecific.gameDirectory || "undefined";
    }

    close() {
        this.$store.commit('closeGameRunningModal');
    }

    get isOpen(): boolean {
        return this.$store.state.modals.isGameRunningModalOpen;
    }
}
</script>
