<script>
  // @ts-nocheck

  import { apiCallCurrent, apiCallforecast } from "./services";
  import { ImageWeather } from "./middleware/imageWeather";
  import { Container, Row, Col } from "sveltestrap";
  import CurrentWeather from "./lib/currentWeather/index.svelte";
  import Detail from "./lib/detailWeather/index.svelte";
  import Forecast from "./lib/forecast/index.svelte";
  import SearchInput from "./lib//searchInput/index.svelte";
  import ErrorWrapper from "./lib/handlingWrapper/errorWrapper.svelte";
  import LoadingWrapper from "./lib/handlingWrapper/loadingWrapper.svelte";

  let loading = false;
  let error = false;
  let showWeather = false;

  let query = "";
  let search = "Kingdom";

  let weather = {};
  let detail = {};
  let forecast = [{}];
  let errorMessage = {};
  let backgroundWeather

  const fetchData = async () => {
    try {
      loading = true;
      const currentWeather = await apiCallCurrent(search);
      const forecastList = await apiCallforecast(search);
      weather = {
        id: currentWeather.data.weather[0].id,
        name: currentWeather.data.name,
        description: currentWeather.data.weather[0].description,
        icon: currentWeather.data.weather[0].icon,
        temp: currentWeather.data.main.temp,
        timezone: currentWeather.data.timezone,
      };
      backgroundWeather = ImageWeather(weather.id);
      detail = {
        clouds: currentWeather.data.clouds.all,
        humidity: currentWeather.data.main.humidity,
        wind: currentWeather.data.wind.speed,
        sunrise: currentWeather.data.sys.sunrise,
        sunset: currentWeather.data.sys.sunset,
        timezone: currentWeather.data.timezone,
      };
      forecast = forecastList.data.list;

      loading = false;
      showWeather = true;
      error = false;
    } catch (e) {
      loading = false;
      errorMessage = e.response.data;
      loading = false;
      error = true;
    }
  };

  $: if (search !== "") {
    fetchData();
  }

  const handleChange = (event) => (query = event.target.value);

  const handleSubmit = (event) => {
    event.preventDefault();
    search = query;
    query = "";
  };
</script>

<main>
  <div class="App" style:background={backgroundWeather}>
    <Container class="mt-4">
      <SearchInput {query} {handleChange} {handleSubmit} />
      {#if loading}
        <LoadingWrapper />
      {/if}
      {#if !loading && showWeather}
        <Row class="animate__animated animate__fadeIn">
          <Col lg={6} xs={12} class="mt-3">
            <CurrentWeather {...weather} />
          </Col>
          <Col lg={6} xs={12} class="mt-3">
            <Detail {...detail} />
          </Col>
          <Col lg={12} class="mt-3">
            <Forecast {forecast} />
          </Col>
        </Row>
      {/if}
      {#if !loading && error}
        <div class="animate__animated animate__fadeIn">
          <ErrorWrapper {...errorMessage} />
        </div>
      {/if}
    </Container>
  </div>
</main>
