<template class="body-style">
    <div>

        <div class="ligne-style">
            <div>
                <h1 class="style-title">Today, 08 Oct </h1>
            </div>
            <div class=""></div>
            <button @click="stopTimer" :class="{ 'stop-button': isRunning, 'start-button': !isRunning }"
                :disabled="!isRunning">
                <span class="stop-icon"></span>Stop
            </button>
            <button class="start-button" @click="startTimer" :disabled="isRunning">
                <font-awesome-icon icon="stopwatch" class="start-icon"></font-awesome-icon> ⏱️ Start new
            </button>

            <span :class="{ 'chro-style': isRunning }">
                {{ formattedTime }}</span>
        </div>
        <div>
            <!-- Votre contenu existant -->
            <h1></h1>

            <!-- Ligne verte -->
            <div class="green-line"></div>
            <div class="layout">
                <div class=" period"> Period 3 <span clase=" "> 4:00 pm </span>
                    <span class="{'chro-enre'}}"> {{ formattedTime }}
                    </span>
                    <div class="CP"> Current Period </div>
                    <div>Period <span clase=" "> 4:00 pm </span>
                        <span class="{'chro-enre'}}"> {{ formattedTime }}

                        </span>
                    </div>
                    <div class="green-line"></div>
                </div>
            </div>

        </div>
    </div>
</template>

<script lang="ts">
export default {

    data() {
        return {
            isRunning: false,
            startTime: null as Date | null, // Type Date ou null
            endTime: null as Date | null, // Type Date ou null
            chronomètresEnregistres: [] as { startTime: Date | null, endTime: Date | null }[],
            // Spécifiez le type du tableau 
            DataBase: null,
        };
    },
    computed: {
        formattedTime() {
            if (this.startTime && this.endTime) {
                const elapsedMilliseconds = this.endTime.getTime() - this.startTime.getTime() as number;
                // Convertir en nombre
                const seconds = Math.floor(elapsedMilliseconds / 1000);
                const minutes = Math.floor(seconds / 60);
                const remainingSeconds = seconds % 60;
                const hours = Math.floor(minutes / 60);
                const remainingMinutes = minutes % 60;

                return `${hours.toString().padStart(2, '0')}:${remainingMinutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
            }
            return '00:00:00';
        },
    },
    methods: {
        startTimer() {
            if (!this.isRunning) {
                this.startTime = new Date();
                this.isRunning = true;
                this.updateTime();
            }
        },
        stopTimer() {
            if (this.isRunning) {
                this.endTime = new Date();
                this.isRunning = false;
                // Créez un nouvel objet de chronomètre enregistré
                const chronometreEnregistre = {
                    startTime: this.startTime,
                    endTime: this.endTime,
                };

                // Ajoutez le chronomètre enregistré à la liste
                this.chronomètresEnregistres.push(chronometreEnregistre);

                // Réinitialisez le chronomètre actuel
                this.startTime = null;
                this.endTime = null;
            }
        },
        updateTime() {
            if (this.isRunning) {
                this.endTime = new Date();
                setTimeout(this.updateTime, 1000);
            }
        },
    },
};
</script>
<style scoped > /* Importez le fichier CSS personnalisé */
@import './components/TimeTracker.css';

/* Vos styles CSS existants / / ... */
</style>
