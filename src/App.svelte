<script>
  let items = [{ alcohol: 3, capacity: 350, quantity: 1 }];
  let weight = 60;
  let title = "";
  let contents = "";
  // 項目を追加
  function add() {
    // ES6 のスプレッド構文
    items = [...items, { alcohol: 3, capacity: 350, quantity: 1 }];
  }
  // 項目を削除
  function remove(index) {
    items.splice(index, 1);
    items = items;
  }
  // 純アルコール量計算
  function computeAlcohol(items) {
    let alcohol = 0;
    items.forEach((value) => {
      alcohol += (value.alcohol / 100) * value.capacity * 0.8 * value.quantity;
    });
    return alcohol.toFixed(1);
  }
  // 血中アルコール濃度の計算・酔いの状態を表示
  function computeAlcoholConc(items, weight) {
    let alcoholConc = 0;
    items.forEach((value) => {
      alcoholConc +=
        ((value.capacity * value.quantity * (value.alcohol / 100)) /
          (833 * weight)) *
        100;
    });
    switch (true) {
      case alcoholConc <= 0.01:
        title = "通常時";
        contents = "";
        break;
      case alcoholConc >= 0.02 && alcoholConc <= 0.04:
        title = "爽快期";
        contents = "気分さわやか。活発な態度をとる。";

        break;
      case alcoholConc >= 0.05 && alcoholConc <= 0.1:
        title = "ほろ酔い期";
        contents =
          "ほろ酔い気分。脈拍数、呼吸数早くなる。話はなめらかになり、抑制が外れる。";
        break;
      case alcoholConc >= 0.11 && alcoholConc <= 0.15:
        title = "酩酊初期";
        contents = "気が大きくなり、自己抑制が外れる。立てば少しふらつく。";
        break;
      case alcoholConc >= 0.16 && alcoholConc <= 0.3:
        title = "酩酊期";
        contents =
          "	運動障害が出現する。まともに歩けない。（千鳥足）呼吸促拍、嘔気、嘔吐";
        break;
      case alcoholConc >= 0.31 && alcoholConc <= 0.4:
        title = "泥酔期";
        contents =
          "	歩行困難。転倒すると起きあがれない。意識混濁、言語支離滅裂。";
        break;
      case alcoholConc >= 0.41:
        title = "昏睡期";
        contents = "	昏睡状態。屎尿失禁。呼吸麻痺をきたし、死亡する危険大";
        break;
    }
    return alcoholConc.toFixed(2);
  }
</script>

<main class="bg-white pb-6 sm:pb-8 lg:pb-12 max-w-screen-sm mx-auto">
  <div class="">
    <header
      class="border-b py-4 mt-4 mb:mt-8 md:py-8 mb-8 md:mb-12 xl:mb-16 px-4 md:px-8"
    >
      <div class="flex justify-between items-center  ">
        <h1
          class="inline-flex items-center text-black-800 text-1xl md:text-2xl font-bold gap-2.5"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-9 w-9 text-indigo-500"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"
            />
          </svg>
          純アルコール量・血中アルコール濃度けいさん機
        </h1>
      </div>
      <p class="mt-6 text-xs md:text-sm">
        度数または
        %・摂取量(ml)・数量をそれぞれ入力すると純アルコール量の計算、体重を入力すると血中アルコール濃度が計算され結果が表示されます。+ボタンで項目追加、xボタンで項目を削除できます。
      </p>
    </header>

    <div class="border-b">
      <table class="table-auto mx-auto">
        <thead>
          <tr class="text-sm lg:text-base">
            <th class="px-4 py-2">度数または %</th>
            <th class="px-4 py-2">摂取量(ml)</th>
            <th class="px-4 py-2">数量</th>
          </tr>
        </thead>
        <tbody>
          {#each items as item, index}
            <tr class="bg-gray-100">
              <td class="border px-4 py-2">
                <input
                  class="input"
                  bind:value={item.alcohol}
                  type="number"
                  min="0"
                />
              </td>
              <td class="border px-4 py-2">
                <input
                  class="input"
                  bind:value={item.capacity}
                  type="number"
                  min="0"
                />
              </td>
              <td class="border px-4 py-2">
                <input
                  class="input"
                  bind:value={item.quantity}
                  type="number"
                  min="0"
                />
              </td>
              <td
                class="border px-4 py-2 hover:bg-gray-300 hover:cursor-pointer"
              >
                <span class="" on:click={() => remove(index)}
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M6 18L18 6M6 6l12 12"
                    />
                  </svg></span
                >
              </td>
            </tr>
          {/each}
        </tbody>
      </table>

      <div class="flex justify-center items-center my-4">
        <button
          class="inline-block bg-gray-100 hover:bg-gray-300 active:bg-gray-200 focus-visible:ring ring-indigo-300 text-gray-800 text-sm md:text-base font-semibold text-center rounded-lg outline-none transition duration-100 px-8 py-3"
          on:click={add}
          ><svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4v16m8-8H4"
            />
          </svg></button
        >
      </div>
      <table class="table-auto mx-auto mt-3">
        <thead>
          <tr class="text-sm lg:text-base">
            <th class="px-4 py-2">体重(kg)</th>
          </tr>
        </thead>
        <tbody>
          <tr class="bg-gray-100">
            <td class="border px-4 py-2">
              <input class="input" bind:value={weight} type="number" min="0" />
            </td>
          </tr>
        </tbody>
      </table>
      <div class="text-center text-base lg:text-lg my-9">
        <p class="text-gray-600">
          純アルコール量約 <span class="font-bold"
            >{computeAlcohol(items)}g</span
          >
        </p>
        <p class="text-gray-600">
          血中アルコール濃度約 <span class="font-bold"
            >{computeAlcoholConc(items, weight)}%</span
          >
        </p>
        <p class="mt-5 text-indigo-500 font-semibold">{title}</p>
        <p class="mt-2 text-gray-600">{contents}</p>
      </div>
    </div>
  </div>
</main>

<style>
</style>
