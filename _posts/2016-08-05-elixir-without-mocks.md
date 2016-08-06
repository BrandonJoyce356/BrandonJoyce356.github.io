---
layout: post
title: Elixir Without Mocks - A Succinct Example
permalink: elixir-without-mocks
---
Wrapping my head around why mocking is less favored in Elixir than in Ruby (and other languages).
Here's an example asserting that a function is called and matching the arguments without any mocking tools.

{% highlight elixir %}
defmodule NoMocksTest do
  use ExUnit.Case

  test "says hello to the console" do
    NoMocks.process(io_stub)
    assert_receive {:puts, "hello"}
  end

  defp io_stub do
    %{ puts: fn(msg) -> send(self(), {:puts, msg}) end }
  end
end

defmodule NoMocks do
  @io %{puts: &IO.puts/1}

  def process(io \\ @io) do
    io.puts.("hello")
  end
end
{% endhighlight %}
