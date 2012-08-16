Validation
=========

Introduction to parameter validation
------------------------------------

Ninja uses hibernates implementation of the <code>javax.validation</code> feature. This means that all JSR303-defined annotations work in your DTO-objects when you let ninja inject parameters into your controller. All you have to do is to set the <code>@JSR303Validation</code> annotation in front of your method-parameter and catch the result of the validation by injecting a <code>Validation</code>-parameter the same way.

For example:

<pre class="prettyprint">
	public Result postConfirmedPublication(
		@ProfileId String profileId,
	    @JSR303Validation Dto dto, Validation validation) {
		...
	}
</pre>

In this example we want the context to be parsed as <code>profileId</code> and a <code>Dto</code>-object. Because of the leading <code>@JSR303Validation</code>-annotation all public fields in <code>Dto</code> may contain JSR303-annotations like this:

<pre class="prettyprint">
	public class Dto {
	        @Pattern(regexp = "[a-z]*")
	        public String regex;
	        @Size(min = 5, max = 10)
	        public String length;
	        @Min(value = 3)
	        @Max(value = 10)
	        public int range;
	}
</pre>

You can check the result of the validation by evaluating the <code>validation</code>-parameter with simply calling its <code>annotation.hasViolations()</code>-method in your controller which gives you a <code>true</code> or <code>false</code>.