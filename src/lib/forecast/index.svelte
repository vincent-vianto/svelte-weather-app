<script>
  import "./style.css";
  import moment from "moment";
  import { icons } from "../../middleware/importAssets";
  export let forecast;

  const dayPicker = (time) => {
    const dateNow = moment(Date.now());
    const dateDt = moment(time);
    let dayDiff =
      dateNow.diff(dateDt, "day") === 0
        ? "Today"
        : dateNow.diff(dateDt, "day") === -1
        ? "Tomorrow"
        : dateDt.format("D MMM");
    return dayDiff;
  };
</script>

<main>
  <div class="mt-3">
    <h1 class="mb-2">Timeline</h1>
    <div class="forecastWrapper">
      {#each forecast as item}
        <div class="forecastBlock">
          <h6>{dayPicker(item.dt_txt)}</h6>
          <h6>{moment(item.dt_txt).format("LT")}</h6>
          <img
            src={icons(`${item.weather[0].icon}.svg`)}
            alt={item.weather[0].icon}
            draggable={false}
            width={100}
            height={100}
          />
          <h6>{item.weather[0].description}</h6>
        </div>
      {/each}
    </div>
  </div>
</main>
