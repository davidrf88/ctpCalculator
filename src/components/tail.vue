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
                            @change="HandelStartingModelChange"
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
                                <td v-if="startingModel != 'custom'" class="p-4">{{ card.startingHole }}</td>
                                <td v-if="startingModel == 'custom'" class="p-4"><select :disabled="appStatus > 2"
                                        v-model="card.startingHole" id="ddlch"
                                        class="block w-20 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                                        <option v-for="c in range(1, noHoles)" :key="c" :value="c">{{ c }}
                                        </option>
                                    </select></td>
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
                    <label class=" grid justify-items-center flex-1 text-2xl font-medium text-gray-700">CTP Assignment <span
                            class=" " :class="className + ' contents'">{{
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

        <section id="Results" v-if="appStatus > 3"
            class="bg-white p-8 mb-8 rounded-lg shadow-md md:w-1/2 w-full max-w-full">
            <form @submit.prevent="submitForm" class="">
                <label class=" grid justify-items-center flex-1 text-2xl font-medium text-gray-700">Results</label>

                <div class="p-4">

                    <div class="container mx-auto mt-2 border-b-2 border-b-indigo-700 border-">
                        <div v-if="results.ctpHolesx1.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">Single CTPs:
                                </label></div>

                        </div>
                        <div v-if="results.ctpHolesx1.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">[{{
                                        results.ctpHolesx1.join(', ') }}]</label>
                            </div>
                        </div>
                        <div v-if="results.ctpHolesx2.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">Double CTPs:
                                </label></div>

                        </div>
                        <div v-if="results.ctpHolesx2.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">[{{
                                        results.ctpHolesx2.join(', ') }}]</label>
                            </div>
                        </div>
                        <div v-if="results.ctpHolesx3.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">Triple CTPs:
                                </label></div>

                        </div>
                        <div v-if="results.ctpHolesx3.length > 0" class="grid grid-cols-1 gap-4">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-indigo-700">[{{
                                        results.ctpHolesx3.join(', ') }}]</label>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 gap-4 ">
                            <div class=""><label
                                    class=" grid justify-items-left flex-1 text-xl font-medium text-orange-700">NO CTPs:
                                </label></div>
                        </div>
                        <div class="grid grid-cols-1 gap-4 ">
                            <div class=""><label class=" grid justify-items-left text-xl font-medium text-orange-700">[{{
                                results.ctpHolesx0.join(', ') }}]</label>
                            </div>
                        </div>
                    </div>
                </div>


                <div id="resultingCards">
                    <div class="p-4">

                        <div v-for="card in cards" class="container mx-auto mt-2 border-b-2 border-b-indigo-700 border-">
                            <!-- First row: Custom column widths -->
                            <div class="grid grid-cols-6 gap-6">
                                <div class="col-span-1 p-4 "><label
                                        class=" grid justify-items-left flex-1 text-xl font-medium text-gray-700">Card</label>
                                </div>
                                <div class="col-span-1 p-4 "><span
                                        class="inline-flex items-start justify-start px-2 py-1 mr-2 text-2xl font-bold leading-none text-indigo-100 bg-indigo-600 rounded-full">{{
                                            card.cardNumber }}</span></div>
                                <div class="col-span-1 p-4 "><label
                                        class=" grid justify-items-left flex-1 text-xl font-medium text-gray-700">Start</label>
                                </div>
                                <div class="col-span-1 p-4"><span
                                        class=" inline-flex items-start justify-start px-2 py-1 mr-2 text-2xl font-bold leading-none text-indigo-600 bg-indigo-100 rounded-full">{{
                                            card.startingHole }}</span></div>

                            </div>

                            <!-- Second row: 2 columns justified left and last column extends -->

                            <div v-if="!card.playingCtps" class="grid grid-cols-1 gap-4">
                                <div class=""><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-orange-700">Card Not playing CTPS</label>
                                </div>
                            </div>


                            <div class="grid grid-cols-3 gap-4">
                                <div class="col-span-1"><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">Flags</label>
                                </div>
                                <div class="col-span-2"><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">[{{ card.ctpFlags.length }}]</label>
                                </div>
                            </div>

                            <!-- Third row: 2 columns justified left and last column extends -->
                            <div class="grid grid-cols-3 gap-4">
                                <div class="col-span-1 "><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">Place</label>
                                </div>
                                <div class="col-span-2 "><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">[{{card.ctpFlags.join(', ')}}]</label>
                                </div>
                            </div>

                            <!-- Fourth row: 2 columns justified left and last column extends -->
                            <div class="grid grid-cols-3 gap-4 mb-2">
                                <div class="col-span-1 "><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">pick
                                        up</label></div>
                                <div class="col-span-2 "><label
                                        class=" grid justify-items-left flex-1 text-lg font-medium text-gray-700">[{{ card.pickupFlags.join(', ') }}]</label>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>


                <div id="navigation2" class="flex mb-5  ">
                    <div class="flex-1 grid justify-items-center">
                        <button type="button" :disabled="appStatus > 4" @click="MoveBackToStep3"
                            class=" w-full py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-orange-400 disabled:bg-gray-400 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-orange-500">
                            Back
                        </button>
                    </div>
                    <div class="flex-1 grid justify-items-center">

                        <button type="button" :disabled="appStatus > 4"
                            class="w-full ml-3 py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 disabled:bg-gray-400 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                            Share
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
            startingModels: [{ name: 'standard' }, { name: 'sequential' }, { name: 'reversed' }, { name: 'custom' }],
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

            ],
            results: {
                ctpHolesx1: [],
                ctpHolesx2: [],
                ctpHolesx3: [],
                ctpHolesx0: []
            }
        };
    },
    computed: {
        assignedCtps: function () {
            return this.holes.reduce((sum, obj) => sum + obj.ctpMultipliers, 0);
        },
        className: function () {
            return this.assignedCtps == this.noCtps ? 'text-green-700' : 'text-red-700';
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
            this.StartingModelStandard();

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
        UnassignAll() {
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
            if (!this.validateCardStartingHoles()) {
                return;
            }

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
                alert('available CTPs not matching assigned CTPs')
            }
            else {
                this.GenerateResults()
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
            this.results =  {
                ctpHolesx1: [],
                ctpHolesx2: [],
                ctpHolesx3: [],
                ctpHolesx0: []
            }
            this.appStatus = 3
        },
        StartingModelStandard() {
            let standardOrders = new Map();
            //TODO algorighm to generate this hardcoded model
            standardOrders.set(18, [1, 3, 5, 7, 9, 11, 13, 15, 17, 16, 14, 12, 10, 8, 6, 4, 2, 18])
            standardOrders.set(19, [1, 3, 5, 7, 9, 11, 13, 15, 17, 16, 14, 12, 10, 8, 6, 4, 2, 18, 19])
            standardOrders.set(20, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 18, 16, 14, 12, 10, 8, 6, 4, 2, 20])
            standardOrders.set(21, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 18, 16, 14, 12, 10, 8, 6, 4, 2, 20, 21])
            standardOrders.set(22, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 20, 18, 16, 14, 12, 10, 8, 6, 4, 2, 22])
            standardOrders.set(23, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 20, 18, 16, 14, 12, 10, 8, 6, 4, 2, 22, 23])
            standardOrders.set(24, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 22, 20, 18, 16, 14, 12, 10, 8, 6, 4, 2, 24])
            standardOrders.set(25, [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 22, 20, 18, 16, 14, 12, 10, 8, 6, 4, 2, 24, 25])

            var modelNumbers = standardOrders.get(this.noHoles);

            var sortedCards = this.cards.sort((a, b) => a.cardNumber - b.cardNumber);
            for (var i = 0; i < sortedCards.length; i++) {
                sortedCards[i].startingHole = modelNumbers[i];

            }

        },
        StartingSequential() {
            var sortedCards = this.cards.sort((a, b) => a.cardNumber - b.cardNumber);
            for (var i = 0; i < sortedCards.length; i++) {
                sortedCards[i].startingHole = i + 1;

            }
        },
        StartingReversed() {
            var sortedCards = this.cards.sort((a, b) => a.cardNumber - b.cardNumber);
            for (var i = 0; i < sortedCards.length; i++) {
                sortedCards[i].startingHole = this.noHoles - i;

            }
        },
        HandelStartingModelChange() {

            if (this.startingModel == 'standard') {
                this.StartingModelStandard();
            }
            if (this.startingModel == 'sequential') {
                this.StartingSequential();
            }
            if (this.startingModel == 'reversed') {
                this.StartingReversed();
            }

        },
        validateCardStartingHoles() {
            var roundHoles = this.range(1, this.noHoles);
            var startingHoles = this.cards.map(x => x.startingHole);
            var valid = true;

            startingHoles.forEach(hole => {
                if (valid) {
                    //validate valid holeNumber
                    if (!roundHoles.includes(hole)) {
                        alert('Hole ' + hole + ' does not exists on this round')
                        valid = false;
                    }
                    //validate no duplicate
                    if (startingHoles.filter(sh => sh === hole).length > 1) {
                        alert('Hole ' + hole + ' is repeated')
                        valid = false;

                    }
                }

            });

            // validate not same alternative
            // let difference = roundHoles.filter(num => !startingHoles.includes(num));
            // if(difference.length > 0)
            // {
            //     alert('Holes missing ' + difference.join(','));
            //     return false;
            // }

            return valid;

        },
        GenerateResults() {
            var roundHoles = this.range(1, this.noHoles);
            var ctpHoles = this.holes.filter(x => x.ctpMultipliers > 0).map(x => x.holeNumber);
            this.results.ctpHolesx1 = this.holes.filter(x => x.ctpMultipliers == 1).map(x => x.holeNumber);
            this.results.ctpHolesx2 = this.holes.filter(x => x.ctpMultipliers == 2).map(x => x.holeNumber);
            this.results.ctpHolesx3 = this.holes.filter(x => x.ctpMultipliers == 3).map(x => x.holeNumber);
            this.results.ctpHolesx0 = this.holes.filter(x => x.ctpMultipliers == 0).map(x => x.holeNumber);
            this.cards.forEach(card => {
                card.ctpFlags = [];
                card.pickupFlags = [];

            });

            var ctpsAssigments = this.calculateCTP(ctpHoles, this.cards, this.noHoles)
            ctpsAssigments.forEach(ctp => {

                var card = this.cards.find(x => x.cardNumber == ctp.cardNumber) || null;
                if (card) {
                    
                    card.ctpFlags.push(ctp.hole);
                }
                else {
                    valid = false;
                    console.log('Invalid CTP Configuration');
                }
            });
            //PICKUP HOLES
           this.CalculatePickupHoles(this.cards)
            


        },

        calculateCTP(ctps, cards, totalHoles) {
            var results = [];
            for (var i = 0; i < ctps.length; i++) {
                let ctpNumber = ctps[i];
                var cardAssigned = this.ctpAssigned(ctpNumber, cards, totalHoles);
                results.push({ hole: ctpNumber, cardNumber: cardAssigned });

                //console.log('CTP flag for hole: '+ ctpNumber + ' will be assigned to card ' + cardAssigned)
            }
            return results;
        },

        ctpAssigned(ctp, cards, totalHoles) {
            var cardNumber = cards[0].cardNumber;
            var minHolesBefore = 99;
            for (var i = 0; i < cards.length; i++) {
                var card = cards[i];
                if (!card.playingCtps) { continue; }


                var holesBeforeCtp = this.GetHolesBeforeCTP(card.startingHole, ctp, totalHoles)
                //console.log(holesBeforeCtp)
                if (holesBeforeCtp < minHolesBefore) {
                    minHolesBefore = holesBeforeCtp;
                    cardNumber = card.cardNumber;
                }
            }
            return cardNumber;
        },

        GetHolesBeforeCTP(startingHole, ctp, numberOfHoles) {
            var currentHole = startingHole;
            var holesBeforectp = 0;
            for (var i = 0; i < 18; i++) {
                if (currentHole == ctp) {
                    return holesBeforectp;
                }
                if (currentHole == 18) {
                    currentHole = 1;
                } else {

                    currentHole++;
                }
                holesBeforectp++;


            }


        },

        CalculatePickupHoles(cards) {


            //THIS LOGIC NEEDS TO BE REWORK
            var ctpHoles = []
            cards.forEach(element => {
                console.log(`adding ${cards.map(x => x.ctpFlags)}`)
                ctpHoles.push(...cards.map(x => x.ctpFlags));
            });

            alert(`pickup holes ${ctpHoles.length} `)
            
            //extract ctp holes
            //for each ctpHole determine whos card needs to pickit up


        },
    },
}

</script>
  