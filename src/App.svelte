<script >
  import { Router, Link, Route } from "svelte-navigator";
  import Private from './pages/Private.svelte'
  import Login from './pages/Login.svelte'
</script>

<Router>
  <nav>
    <Link to="/">Login</Link>
    <Link to="redirect">Private page</Link>
  </nav>
  <div>
    <Route path="/" component={Login} />
    <Route path="/redirect" component={Private} />
  </div>
  </Router>