<script>
   let newItem = '';
   let shoppingList = [];
   let basketList = [];
   let itemId = 0; //loob itemitele unikaalsed id'd - et checkbox viga parandada (liikusid kõik koos)

   function addItem () {
     // console.log(new)
   
      if (newItem.trim() !== '') {
         console.log(shoppingList)
         if(shoppingList.filter(item => item.name.toLocaleLowerCase() === newItem.toLocaleLowerCase()).length){
            return shoppingList
         }
         
         shoppingList = [...shoppingList, {id: itemId++, name: newItem}]; // ... -> using spread prop to create a new reference (muidu ei uuendanud listi)
         newItem = ''; //this cleans the input field
      }
   }

   function keyboardSubmit(event) {
      if (event.key === 'Enter') {
         addItem();
      }
   }

   function moveToBasket(item) {
      if (!basketList.some(i => i.id === item.id)) { //some vaatab ega sama nimega asja juba listis ei ole
         shoppingList = shoppingList.filter(i => i.id !== item.id);
         basketList = [...basketList, item]; //creates a new array reference with spread
      }
   }

   function moveToShoppingList(item) {
      console.log(item);
      if (!shoppingList.some(i => i.id === item.id)) {
         basketList = basketList.filter(i => i.id !== item.id);
         shoppingList = [...shoppingList, item];
      }
   }
</script>

<main>
   <h1 class="text-2xl font-semibold">🥨 My shopping list 🥨</h1>

   <input
      class="rounded-full"
      type="text"
      bind:value={newItem}
      placeholder="Add an item"
      on:keydown={keyboardSubmit}
   /> <!-- poogin siia selle keyboardSubmit event listeneri -->

   <button
      class="bg-blue-500 text-lg text-white font-semibold rounded-full hover:bg-blue-700 transition duration-200"
      on:click={addItem}>
      Add
   </button>

   <h2 class="text-lg font-medium">Shopping list 📋</h2>
   <ul>
      {#each shoppingList as item (item.id)}
         <li>
            <input
               type="checkbox"
               class="rounded"
               on:change={() => moveToBasket(item)}
            />
            {item.name}
         </li>
      {/each}
   </ul>

   <h2 class="text-lg font-medium">In the basket 🛒</h2>
   <ul>
      {#each basketList as item(item.id)}
         <li>
            <input
               type="checkbox"
               class="rounded"
               on:change={() => moveToShoppingList(item)}
               checked
            />
            {item.name}
         </li>
      {/each}
   </ul>
</main>

<style>
   main {
      font-family: ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
      max-width: 500px;
      margin: 20px auto;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 5px;
   }

   h1 {
      text-align: center;
      margin-bottom: 40px;
   }

   h2 {
      margin: 20px auto;
   }

   input[type='checkbox'] {
      margin-right: 8px;
   }

   button {
      padding: 8px 24px;
   }

   ul {
      list-style-type: none;
      padding: 0;
   }

   li {
      display: flex;
      align-items: center;
      margin: 5px 0;
   }

</style>