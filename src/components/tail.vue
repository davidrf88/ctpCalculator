<template>
    <div class="min-h-screen v-full flex flex-col items-center bg-gray-100">
        <h1 class="text-4xl font-bold mb-8">CTP Calculator</h1>

        <section id="CardsAndHoles" class="bg-white p-8 mb-8 rounded-lg shadow-md md:w-1/2 w-full max-w-full">
            <form @submit.prevent="submitForm" class="">
                <div id="selectors" class="flex mb-10  ">
                    <div class="flex-1 grid justify-items-center">
                        <label for="holes" class="block text-md font-medium text-gray-700">Holes</label>
                        <select :disabled="appStatus > 1" v-model="noHoles" id="holes"
                            class="block w-16 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                            <option v-for="n in range(18, 25)" :key="n" :value="n">{{ n }}</option>
                        </select>

                    </div>

                    <div class="flex-1 grid justify-items-center">
                        <label for="cards" class="block text-md font-medium text-gray-700">Cards</label>
                        <select :disabled="appStatus > 1" v-model="noCards" id="cards"
                            class="block w-16 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                            <option v-for="n in range(2, 25)" :key="n" :value="n">{{ n }}</option>
                        </select>
                    </div>

                    <div class="flex-1 grid justify-items-center">
                        <label for="ctps" class="block text-md font-medium text-gray-700">CTPs</label>
                        <select :disabled="appStatus > 1" v-model="noCtps" id="ctps"
                            class="block w-16 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                            <option v-for="n in range(1, 75)" :key="n" :value="n">{{ n }}</option>
                        </select>
                    </div>


                </div>
                <div>
                    <button :disabled="appStatus > 1" type="button" @click="MoveToStep2"
                        class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 disabled:bg-gray-400 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        Next
                    </button>
                </div>
            </form>
        </section>


        <section id="StartingModel" v-if="appStatus > 1"
            class="bg-white p-8 mb-8 rounded-lg shadow-md md:w-1/2 w-full max-w-full">
            <form @submit.prevent="submitForm" class="">
                <div id="selectors2" class="flex mb-5  ">
                    <div class="flex-1 grid justify-items-center">
                        <label for="holes" class="block text-md font-medium text-gray-700">Starting model</label>
                        <select :disabled="appStatus > 2" v-model="startingModel" id="holes"
                            class="block w-40 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                            <option v-for="n in startingModels" :key="n.name" :value="n.name">{{ n.name }}</option>
                        </select>

                    </div>

                </div>
                <div id="cardsGeneration" class="mb-10">
                    <label class=" grid justify-items-center flex-1 text-2xl font-medium text-gray-700">Cards</label>
                    <table class="table-auto w-full">
                        <thead>
                            <tr class="border-b border-indigo-600 text-indigo-600">
                                <th class="p-4 text-left">Card</th>
                                <th class="p-4 text-left">Hole</th>
                                <th class="p-4 text-left">Playing CTP</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="card in cards" class="border-b border-indigo-600">
                                <td class="p-4">{{ card.cardNumber }}</td>
                                <td class="p-4">{{ card.startingHole }}</td>
                                <td><input :disabled="appStatus > 2" type="checkbox" v-model="card.playingCtps"
                                        class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <div id="navigation2" class="flex mb-5  ">
                    <div class="flex-1 grid justify-items-center">
                        <button type="button" @click="MoveBackToStep1" :disabled="appStatus > 2"
                            class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-orange-400 disabled:bg-gray-400 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-orange-500">
                            Back
                        </button>
                    </div>
                    <div class="flex-1 grid justify-items-center">

                        <button type="button" :disabled="appStatus > 2" @click="MoveToStep3"
                            class="w-full ml-3 py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 disabled:bg-gray-400 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                            Next
                        </button>
                    </div>
                </div>
            </form>

        </section>

        <section id="CtpAssigment" v-if="appStatus > 2"
            class="bg-white p-8 mb-8 rounded-lg shadow-md md:w-1/2 w-full max-w-full">
            <form @submit.prevent="submitForm" class="">
                <div id="ctpAssignmentx" class="mb-10">
                    <label class=" grid justify-items-center flex-1 text-2xl font-medium text-gray-700">CTP Assignment <span class=" " :class="className + ' contents'" >{{
                        assignedCtps }} </span> of {{ this.noCtps }} </label>
                    <div id="navigationx" class="flex mb-5 mt-5  ">
                        <div class="flex-1 grid justify-items-center">
                            <button type="button" :disabled="appStatus > 3" @click="UnassignAll"
                                class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-orange-400 disabled:bg-gray-400 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-orange-500">
                                Unassign all
                            </button>
                        </div>
                    </div>
                    <table class="table-auto w-full">
                        <thead>
                            <tr class="border-b border-indigo-600 text-indigo-600">
                                <th class="p-4 text-left">Hole</th>
                                <th class="p-4 text-left">CTP</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="hole in holes" class="border-b border-indigo-600">
                                <td class="p-4">{{ hole.holeNumber }}</td>
                                <td>
                                    <select :disabled="appStatus > 3" v-model="hole.ctpMultipliers" id="ctps"
                                        class="block w-28 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                                        <option v-for="c in ctpMultipliers" :key="c.val" :value="c.val">{{ c.name }}
                                        </option>
                                    </select>
                                </td>


                            </tr>
                        </tbody>
                    </table>
                </div>
                <div id="navigation2" class="flex mb-5  ">
                    <div class="flex-1 grid justify-items-center">
                        <button type="button" :disabled="appStatus > 3" @click="MoveBackToStep2"
                            class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-orange-400 disabled:bg-gray-400 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-orange-500">
                            Back
                        </button>
                    </div>
                    <div class="flex-1 grid justify-items-center">

                        <button type="button" :disabled="appStatus > 3" @click="MoveToStep4"
                            class="w-full ml-3 py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 disabled:bg-gray-400 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                            Next
                        </button>
                    </div>
                </div>
            </form>

        </section>

        <section id="Results" v-if="appStatus > 3" class="bg-white p-8 mb-8 rounded-lg shadow-md md:w-1/2 w-full max-w-full">
            <form @submit.prevent="submitForm" class="">
                <h1>To be created</h1>
                <div id="navigation2" class="flex mb-5  ">
                    <div class="flex-1 grid justify-items-center">
                        <button type="button" :disabled="appStatus > 4" @click="MoveBackToStep3"
                            class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-orange-400 disabled:bg-gray-400 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-orange-500">
                            Back
                        </button>
                    </div>

                </div>
            </form>
        </section>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            appStatus: 1,
            noHoles: 18,
            noCards: 5,
            noCtps: 5,
            ctpValue: 5,
            startingModel: 'standard',
            startingModels: [{ name: 'standard' }, { name: 'sequential' }, { name: 'reversed' }],
            ctpMultipliers: [
                { name: 'none', val: 0 },
                { name: 'single', val: 1 },
                { name: 'double', val: 2 },
                { name: 'triple', val: 3 }
            ],
            cards: [

            ],
            holes: [
                { holeNumber: 1, ctpMultipliers: 1 },
                { holeNumber: 2, ctpMultipliers: 1 },
                { holeNumber: 3, ctpMultipliers: 1 },
                { holeNumber: 4, ctpMultipliers: 1 },
                { holeNumber: 5, ctpMultipliers: 0 },

            ]
        };
    },
    computed: {
        assignedCtps: function() {
          return this.holes.reduce((sum, obj) => sum + obj.ctpMultipliers, 0);
        },
        className: function() {
          return this.assignedCtps  == this.noCtps ? 'text-green-700' : 'text-red-700';
          //return 'text-green-700';
        }
      },
    methods: {
        range(start, end) {
            return Array.from({ length: end - start + 1 }, (_, i) => i + start);
        },
        submitForm() {
        
        },
        GenerateCards() {
            this.cards = [];
            for (var i = 0; i < this.noCards; i++) {
                this.cards.push({ cardNumber: i + 1, startingHole: i + 1, playingCtps: true })
            }

        },
        GenerateHoles() {
            this.holes = [];
            for (var i = 0; i < this.noHoles; i++) {
                if (this.assignedCtps < this.noCtps) {

                    this.holes.push({ holeNumber: i + 1, ctpMultipliers: 1 })
                   
                }
                else {
                    this.holes.push({ holeNumber: i + 1, ctpMultipliers: 0 })
                }
            }
            var cardIterator = 0;
            var antiExplode = 0;
            while (this.assignedCtps < this.noCtps) {
                if (antiExplode > 120) {
                    alert('Error')
                    this.MoveBackToStep2()
                    return;
                }

                if ((cardIterator + 1) > this.holes.length) { cardIterator = 0; }

                if (this.holes[cardIterator].ctpMultipliers == 3) {
                    {
                        alert(`Ctps canoot be more than ${this.holes.length * 4} when Holes are ${this.holes.length}`)
                        this.MoveBackToStep2()
                        return;
                    }
                }
                this.holes[cardIterator].ctpMultipliers++;

                cardIterator++;
                antiExplode++;
            }
        },
        UnassignAll(){
            this.holes.forEach(hole => {
                    hole.ctpMultipliers = 0
            });
        },
        MoveToStep2() {
            if (this.noHoles > 17 && this.noCards > 1 && this.noCards <= this.noHoles && this.noCtps > 0) {
                this.GenerateCards();
                this.appStatus = 2;
            }
            else {
                alert("Invalid Inputs");
            }
        },
        MoveToStep3() {
            if (this.cards.length > 1 && this.cards.filter(obj => obj.playingCtps).length > 1) {
                this.GenerateHoles();
                this.appStatus = 3;
            }
            else {
                alert("at least 2 cards needs to be playing CTP");
            }
        },
        MoveToStep4() {
            if (this.assignedCtps != this.noCtps) {
                alert('please assign all ctps')
            }
            else {
                this.appStatus = 4;
            }
        },
        MoveBackToStep1() {
            this.cards = [];
            this.startingModel = "standard"
            this.appStatus = 1;
        },
        MoveBackToStep2() {
            this.holes = [];
            this.appStatus = 2;
        },
        MoveBackToStep3() {
            this.appStatus = 3
        },
    },
};
</script>
  