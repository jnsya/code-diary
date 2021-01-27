# Environment Variables
- A variable whose value is set outside the application.
- An environment variable is defined by the operating system your application is running on, not the application's code itself.
- The purpose of environment variables is to decouple configuration from the application.
  - Any value that can change between deployments - eg credentials for external services - should be extracted into environment variables (and thus decoupled from your application itself).
- Examples of environment variables:
  - Setting the environment for the application with `RACK_ENV` or `RAILS_ENV`
  
- `ENV` or `env`: print list of environment variables on the command line.

## Environment Variables in Ruby
- `ENV` is a variable which behaves like a hash containing every available environment variable.
  - eg access the RACK_ENV with `ENV.fetch("RACK_ENV")` or `ENV["RACK_ENV"]`
  
- *Snapshotting:* Environment Variables are set when you begin a process. If you later change the environment variable outside the process, the running process won't be affected.
- *Closed environment*: changing an environment variable within the process won't change it outside the process.
  - so if you manually change an environment variable in your running Ruby application - e.g. `ENV["PATH"]="some dumb path"` - this won't change the value of `$PATH` in your terminal.

2 ways to set an environment variable:
  1. One-time use inside an application: `API_KEY=2 irb` starts a REPL with the environment variable `API` set to the value of 2.
  1. Set an environment variable for current terminal session: `export API_KEY=2`
  
- The `dotenv` gem allows you to set environment variables from a file called `.env`
  - This is more convenient than setting them manually (using one of the two methods above).
  

## Sources:
- [Ruby environment variables](https://www.rubyguides.com/2019/01/ruby-environment-variables/)
- 
