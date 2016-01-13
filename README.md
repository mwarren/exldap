# ExldapPlaying with eldap from Elixir## InstallationThe package can be installed as:  1. Add exldap to your list of dependencies in `mix.exs`:```elixir        def deps do          [{:exldap, git: "https://github.com/jmerriweather/exldap.git"}]        end```  2. Ensure exldap is started before your application:```elixir        def application do          [applications: [:exldap]]        end```  3. Optionally add 'config\config.secret.exs' file with:```elixir        use Mix.Config        config :exldap, :settings,          server: <server address>,          base: "DC=example,DC=com",          port: 636,          ssl: true,          user_dn: <user distinguished name>,          password: <password>```### Usage with configuration set in config.exs```elixir{:ok, connection} = Exldap.connect{:ok, search_results} = Exldap.search_field(connection, "cn", "test123"){:ok, first_result} = search_results |> Enum.fetch(0)result = Exldap.search_attributes(first_result, "displayName")```### Usage without configuration```elixir{:ok, connection} = Exldap.connect("SERVERADDRESS", 636, true, "CN=test123,OU=Accounts,DC=example,DC=com", "PASSWORD"){:ok, search_results} = Exldap.search_field(connection, "OU=Accounts,DC=example,DC=com", "cn", "useraccount"){:ok, first_result} = search_results |> Enum.fetch(0)result = Exldap.search_attributes(first_result, "displayName")```### Verify credentials with configuration set in config.exs```elixir{:ok, connection} = Exldap.opencase Exldap.verify_credentials(connection, "CN=test123,OU=Accounts,DC=example,DC=com", "PASSWORD") do  :ok -> IO.puts "Successfully connected"  _ -> IO.puts "Failed to connect"end```### Verify credentials without configuration```elixir{:ok, connection} = Exldap.open("SERVERADDRESS", 636, true)case Exldap.verify_credentials(connection, "CN=test123,OU=Accounts,DC=example,DC=com", "PASSWORD") do  :ok -> IO.puts "Successfully connected"  _ -> IO.puts "Failed to connect"end```