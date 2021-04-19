<template>
    <div>
    <b-navbar toggleable="lg" type="dark" variant="info">
        <b-collapse id="nav-collapse" is-nav>
            <b-navbar-nav>
                <b-nav-item>Quiz App</b-nav-item>
                <b-nav-item>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
            </b-navbar-nav>
        </b-collapse>
    </b-navbar>
</div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>
