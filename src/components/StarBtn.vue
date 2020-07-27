<template>

<div class="starbutton__container">
    <span class='starbutton__text'>Скопировать записи на другой день недели:</span>
    <div draggable="true" class="button starbutton"></div>
</div>

</template>

<script>

export default {
  name: 'StarBtn'
}

</script>


<style>

.starbutton__text {
    padding: 8px;
    max-width: 190px;
    font-family: 'Exo 2', sans-serif;
    font-size: 16px;
    line-height: 18px;
    height: 50px;
    text-align: right;

    background-color: #E8F5E9;
    box-sizing: border-box;
    border-radius: 10px;
    box-shadow: 0px 0px 5px 1px rgba(0,0,0,0.75);
}

.starbutton__container {
    width: 100%;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    height: 50px;
    width: 100%;
}

.starbutton {
    margin-left: 15px;
    width: 35px;
    height: 35px;
    background-size: contain;
    cursor: pointer;
    background-image: url(https://image.flaticon.com/icons/svg/3004/3004112.svg);
}

</style>