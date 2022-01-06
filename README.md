# PYTHON TRAINING: 
# <h1 style="color:lightblue">Pytest-bdd:<h1>
* ### Installation <span style="color:#fa77ec">=></span>
> *`pip install pytest-bdd` in terminal.*

* ### File <span style="color:#fa77ec">=></span>
> *file extention should be `.feature`.*
	
>*<span style="color:#fa77ec">=></span>each file should only have one `Feature`.*

>*<span style="color:#fa77ec">=></span>each file should have only one `Background`: it is a multi line string that define a certain state to reach in order to perform our test.*

>*<span style="color:#fa77ec">=></span>each file can have more than one scenario.* 

>*<span style="color:#fa77ec">=></span>each `Scenario` should have only one of each (`Given`, `When`, `Then`, `And`)it is a description about a tst case describes what the user would do.*

>*<span style="color:#fa77ec">=></span>the `Given` is a one line string and should define the initial state of the test case.*

>*<span style="color:#fa77ec">=></span>the `When` is a one line string and should state the action to be done by the user.*



# <h1 style="color:lightgreen">Pytest:<h1>

## Configuration:

* ### Installation <span style="color:#fa77ec">=></span>
	> `pip install -U pytest`

* ### We can set pytest Configs in many ways =>
	**1.** *`pytest.ini` file takes precedence over all other files with **`[pytest]`** tag.*

	**2.** *`pyproject.toml` file should contain **`[tool.pytest.ini_options]`** to be considered as a config file.*
	
	**3.** *`tox.ini` file is a tox project config file but can also hold pytest configs if they have **`[pytest]`** tag.*
	
	**4.** *`setup.cfg` file which is a general propose file and can hold pytest configuration if it have **`[tool:pytest]`** tag.*

* ### File Structure =>
	**I.** *Test outside application:*
	```
	setup.py
	mypkg/
		__init__.py
		app.py
		view.py
	tests/
		test_app.py
		test_view.py
		...
	```

	**II.** *As part of application:*
	*`We need to use `--pyargs` to run the test inside the package:*
	##### then pytest will search for that package and collect the tests from there
		```
		setup.py
		mypkg/
			__init__.py
			app.py
			view.py
			test/
				__init__.py
				test_app.py
				test_view.py
				...
		```

* ### Global Variables =>
	> *They should be defined in the **"conftest.py"** file.*

	##### **I.** Use `collect_ingore/collect_ignore_glob = ["file/s_name"]` to ignore test directories or modules.

	##### **II.** Use `pytest_plugins = "plugin"/("plugins")` to register additional plugins.

	##### **III.** Use `pytestmark = single_mark/[list_of_marks]` to apply marks to all test functions in a module.
<br>

* ### Environment Variables =>
	*We set these vars to change pytest behavior:*
	> #### **I.**  PYTEST_ADDOPTS: 
		> *Should contain a command-line that will be propended to the command that we want to run.*
	<br>

	> #### **II.**  PYTEST_CURRENT_TEST: 
		> *This is not meant to be edited but is set with the same name so other processes can inspect it.*
	<br>
	
	> #### **III.**  PYTEST_DEBUG: 
		> *To print tracing and debugging information.*
	<br>
	
	> #### **VI.**  PYTEST_DISABLE_PLUGIN_AUTOLOAD: 
		> *To disable plugins autoload through setuptool entrypoint.*
	<br>
	
	> #### **V.**  PYTEST_PLUGINS: 
		> *Comma separated list of plugins that should be loaded*
	<br>
	
	> #### **IV.**  PY_COLORS: 
		> *`Takes precedence of NO_COLOR` it takes binary values only if `1` it will print colored output, else will never use colors in output.*
<hr>

## Customizations:

* ### `pytest_generate_tests` hook:
	> *Is to implement our own custom parameterization or inhance the existing one.*

	* **=>** *We can change inputs or scope of a fixture for parametrization.*

	* **=>** *We must pass it `metafunc` as its argument to inspect requesting test context.*

	* **=>** *We must call `metafunc.parametrize()` to cause parameterization.*

* ### Exceptions <span style="color:#fa77ec">=></span>
	> *We can custom Exception errors if needed to use them in case of having error to make the error more readable and understandable.*

* ### Warnings <span style="color:#fa77ec">=></span>
	> *We can generate custome warnings for improper usage or deprecated feature.*

* ### Tox <span style="color:#fa77ec">=></span>
	> *Is a virtualenv test automation tool which help us to setup our virtualenv environments and it will run tests recording to the package not the code checkout.*


* ### Custom markers <span style="color:#fa77ec">=></span>
	* **"pytest.ini"** file <span style="color:#fa77ec">=></span>
		```
			[pytest]
			markers =
				low: marks tests as slow (deselect with '-m "not slow"')
				serial
		```

	* **"pyproject.toml"** file <span style="color:#fa77ec">=></span>
		```
			[tool.pytest.ini_options]
			markers = [
				"slow: marks tests as slow (deselect with '-m \"not slow\"')",
				"serial",
				]
		```

	* `pytest_configure` hook <span style="color:#fa77ec">=></span>
		```
		def pytest_configure(config):
			config.addinivalue_line(
				"markers", "env(name): mark test to run only on named environment"
			)
		```

	* **same test file before the function** <span style="color:#fa77ec">=></span>
			```
			@pytest.mark.timeout(10, "slow", method="thread")
			@pytest.mark.slow
			def test_function():
				...```



<hr>

## Commands:

* ### `python -m pytest [options]` <span style="color:#fa77ec">=></span>
	```python -m pytest ..dir../file.py```

* ### `--maxfail="number"` <span style="color:#fa77ec">=></span>
	*To set number of retries.*

* ### `--trace` <span style="color:#fa77ec">=></span>
	*To debug the test from the start.*

* ### Command Line Flags <span style="color:#fa77ec">=></span>
	* you can write `pytest --help` in terminal.*

* ### Using `-r{char}` <span style="color:#fa77ec">=></span>
	*To print reports after a test with certain command:*
	```
		f - failed
		E - error
		s - skipped
		x - xfailed
		X - xpassed
		p - passed
		P - passed with output
		a - all except pP
		A - all
		N - none, this can be used to display nothing (since fE is the default)
	```

* ### Using `pip install pytest-"NAME"`
	> *To install pytest packages and we can write the virtual environment name instead of `pip` for this we are using `pipenv`.*
	
	**1.** we have to add `pytest_plugins` variable with all the plugins we added in the *conftest.py* file.

	**2.** we can disable plugins py writing `-p no:NAME`. 

	**3.** we can check on active plugins by writing `--trace-config`.

	**4.** to disable plugins in CI server we can set `PYTEST_ADDOPTS=-p no:NAME` variable in the env file.
		
	**5.** `pytest-"cov"`: coverage reporting, compatible with distributed testing

	**6.** `pytest-"xdist"`: to distribute tests to CPUs and remote hosts, to run in boxed mode which allows to survive segmentation faults, to run in `looponfailing` mode, automatically re-running failing tests on file changes.

	**7.** `pytest-"instafail"`: to report failures while the test run is happening.

	**8.** `pytest-"bdd"`: to write tests using behavior-driven testing.

	**9.** `pytest-"timeout"`: to timeout tests based on function marks or global definitions.
<hr>

## principles 

* ### Using `raise` <span style="color:#fa77ec">=></span>
	> assert certain exception

* ### Class <span style="color:#fa77ec">=></span>
	> **we can group multiple tests in a class:**
	```
	class TestClass: 
		def test_one(self, `some args`): 
			x = "this"
			assert "h" in x

		def test_two(self, `some args`): 
			x = "hello"
			assert hasattr(x, "check")
	```

* ### Using `tmpdir` <span style="color:#fa77ec">=></span>
	> *We can request a temporary directory for func tests __arbitrary resources__:*
	```	
	def test_needsfiles(tmpdir):
		print(tmpdir)
		assert 0
	```

* ### Using `assert` <span style="color:#fa77ec">=></span>
	> *Used to assert inside a function and we can use multiple assertions inside each function we can also declare a message in case of failure.*
	```
	def test_one(x):
		assert x == 4

	def test_with_message(x): 
		assert x % 2 == 4, "Value should be even, but value was odd"
	```

* ### Using `{excinfo}` <span style="color:#fa77ec">=></span>
	> *To assert on exceptions.*
	```	
	def test_recursion_depth():
		with pytest.raises(RuntimeError) as excinfo:
			def f():
				f()
			f()
		assert "maximum recursion" in str(excinfo.value)
	```

* ### Using `@pytest.mark` (MARKERS) <span style="color:#fa77ec">=></span>
	> *Using it help us to create metadata (test data) for our tests and it only works on tests but not fixtures.*

	* **`usefixtures`** <span style="color:#fa77ec">=></span> *will use fixtures on a test function or a class*
		*`@pytest.usefixture("fixture_name")`*

	* **`filterwarnings`** <span style="color:#fa77ec">=></span> *will filter certain warning of a test function.*
		*`@pytest.mark.filterwarnings("ignore:.*usage will be deprecated,DeprecationWarning")`*

	* **`skip`** <span style="color:#fa77ec">=></span> *will skip a test function.*
		*`@pytest.skip("reason_for_skipping_this_function")`*

	* **`skipif`** <span style="color:#fa77ec">=></span> *will skip a test function under a certain condition*
		*`@pytest.skipif("condition", "reason")`*

	* **`xfail`** <span style="color:#fa77ec">=></span> *will produce expected failure outcome if a certain condition is met*
		*`@pytest.xfail("condition", "reason", "raises: Type[Exception]", "run: Bool", "strict: Bool")`*

	* **`parameterize`** <span style="color:#fa77ec">=></span> *will call a function multiple times for different conditions*
	> *Its better for something like documenting unfixed bugs (where the test describes what “should” happen) or bugs in dependencies.*	

	*`@pytest.parametrize("string", [list])`*

* ### Using `@pytest.fixtures` (FIXTURES) <span style="color:#fa77ec">=></span>
	> *A fixture is a normal function but has the **`@pytest.fixtures`** decorator before it.*

	<span style="color:#fa77ec">=></span> *They provide a fixed baseline data to execute our tests reliably and produce consistent results.*
	
	<span style="color:#fa77ec">=></span> *We can call multiple fixtures in multiple fixtures that depend on them.*
	
	<span style="color:#fa77ec">=></span> *Their return values are cached so we can call them more than once per test.*
	
	<span style="color:#fa77ec">=></span> *We can write fixtures in the **`conftest.py`** file and scope it to:*

	* **`function`**: the default scope, the fixture is destroyed at the end of the test.

	* **`class`**: the fixture is destroyed during teardown of the last test in the class.

	* **`module`**: the fixture is destroyed during teardown of the last test in the module.

	* **`package`**: the fixture is destroyed during teardown of the last test in the package.

	* **`session`**: the fixture is destroyed at the end of the test session.

	* **`function name`**: we can put a certain function name in the scope of a fixture and it will be destroyed after the function ends.
	
	
	<span style="color:#fa77ec">=></span> *Some times when a test fails, its not necessary due to a test problem, there might be a problem in the fixture, then the test requirements won't be completed.*
	
	<span style="color:#fa77ec">=></span> *Because we always have fixtures and there will be too many of them in our tests we cant just leave them there taking more space and bloating the system so we need to clean them up and we can make them clean them up in two ways:*

	<span style="color:#fa77ec">=></span> *Adding **`yield`** instead of **`return`**, then we add the teardown functionality after the yield:*
	```
	from cucumber import CucumberBasket

	@pytest.fixture
	def main_basket():
		return CucumberBasket()

	@pytest.fixture
	def adding_cucbmers(main_basket):
		new = main_basket.add(5)
	yield new
	*	main_basket.empty()
	
	@pytest.fixture
	def eating_cucbmers(adding_cucbmers):
		eaten = adding_cucbmers.eat(3)
	*	yield eaten
	*	main_basket.empty()

	def test_basket(adding_cucbmers, eating_cucbmers): 
		assert adding_cucumbers.count() == 5
		assert eating_cucumbers.count() == 2
	```

	<span style="color:#fa77ec">=></span> *Adding **`addfinalizer`** to the test "request" context object which is longer than **`yield`**:*
	```					
	from cucumber import CucumberBasket

	@pytest.fixture
	def main_basket():
		return CucumberBasket()

	@pytest.fixture
	def adding_cucbmers(main_basket):
		new = main_basket.add(5)

	*	def clear_basket():
	*		main_basket.empty()

	*	request.addfinalizer(clear_basket)
		return new
	
	@pytest.fixture
	def eating_cucbmers(adding_cucbmers):
		eaten = adding_cucbmers.eat(3)

	*	def clear_basket():
	*		main_basket.empty()

	*	request.addfinalizer(clear_basket)
		return eaten

	def test_basket(adding_cucbmers, eating_cucbmers): 
		assert adding_cucumbers.count() == 5
		assert eating_cucumbers.count() == 2
	```
	
	<span style="color:#fa77ec">=></span> *I have to say that in case we wanted to safely clean our fixtures we need to ba careful and precise , any error in the wrong spot will leave some uncleaned data behind. we can use the **`yield`** or **`addfinalizer`** to do it safely.*

	<span style="color:#fa77ec">=></span> *The best fixture structure is to limit a fixture functionality to perform only one action per fixture and then bundling with their teardown(cleanup):*
	```
	@pytest.fixture
	def adding_cucbmers(main_basket):
		new = main_basket.add(5)
		yield new
		main_basket.empty()
	```
	
	<span style="color:#fa77ec">=></span> *Fixtures are only available when they are in the same scope or they are defined as a module **public**
	fixture any other than that it will follow the scope rules.*

	<span style="color:#fa77ec">=></span> *When running fixture pytest runs scoped fixtures at first and the dependencies fixtures comes 2nd:*

	* <span style="color:#22aaec">=></span>*Higher scope fixtures comes first in order <span style="color:#aaffcc">=></span> (`session` - `package` - `module` - `class` - `function`).*

	* <span style="color:#22aaec">=></span>*In multi fixture in the same scope the `autouse` fixtures will run first.*

	<span style="color:#fa77ec">=></span> *We can run multi assertions safely.*

	<span style="color:#fa77ec">=></span> *If there was an action that kick off multi behaviors, we need to set that action fixture as an `autouse` fixture and other behaviors that follow that action can be scoped or normal fixtures.*

	<span style="color:#fa77ec">=></span> *We can use the request and all its functions in fixtures.*

	<span style="color:#fa77ec">=></span> *We can also use markers to pass data to fixtures:*
		```
			@pytest.fixture
			def fixt(request):
		*		marker = request.node.get_closest_marker("fixt_data")
		*		if marker is None:
		*			# Handle missing marker in some way...
		*			data = None
		*		else:
		*			data = marker.args[0]
		*		# Do something with the data
				return data

		*	@pytest.mark.fixt_data(42)
			def test_fixt(fixt):
				assert fixt == 42
		```

	<span style="color:#fa77ec">=></span> *When we need to the same result multiple times we use the "factory as fixture" pattern it returns a function instead of a direct value and the we can call it when ever we need it:*
	```
			@pytest.fixture
		*	def make_customer_record():

				created_records = []

		*		def _make_customer_record(name):
					record = models.Customer(name=name, orders=[])
					created_records.append(record)
					return record

		*		yield _make_customer_record

				for record in created_records:
					record.destroy()


			def test_customer_records(make_customer_record):
		*		customer_1 = make_customer_record("Lisa")
				customer_2 = make_customer_record("Mike")
				customer_3 = make_customer_record("Meredith")
	```

	<span style="color:#fa77ec">=></span> *When we scope fixtures Pytest groups fixtures by their scope, we can consider it like a nested loop where it runs the higher scope fixtures once for all the lower ones:*
	```
		@pytest.fixture(scope="module", params=["mod1", "mod2"])
		def modarg(request):
			param = request.param
			print("  SETUP modarg", param)
			yield param
			print("  TEARDOWN modarg", param)

		@pytest.fixture(scope="function", params=[1, 2])
		def otherarg(request):
			param = request.param
			print("  SETUP otherarg", param)
			yield param
			print("  TEARDOWN otherarg", param)

		def test_2(otherarg, modarg):
			print("  RUN test2 with otherarg {} and modarg {}".format(otherarg, modarg))
	```
	* <span style="color:lightgreen">OUTPUT:</span>
		```	
				collecting ... collected 8 items

			*	test_module.py::test_2[mod1-1]   SETUP otherarg 1
					RUN test2 with otherarg 1 and modarg mod1
				PASSED  TEARDOWN otherarg 1

			*	test_module.py::test_2[mod1-2]   SETUP otherarg 2
					RUN test2 with otherarg 2 and modarg mod1
				PASSED  TEARDOWN otherarg 2

			*	test_module.py::test_2[mod2-1]   SETUP otherarg 1
					RUN test2 with otherarg 1 and modarg mod2
				PASSED  TEARDOWN otherarg 1

			*	test_module.py::test_2[mod2-2]   SETUP otherarg 2
					RUN test2 with otherarg 2 and modarg mod2
				PASSED  TEARDOWN otherarg 2
		```

	<span style="color:#fa77ec">=></span> We can use **`@pytest.mark.usefixture("fixture_name")`** to require fixtures to a certain function.

	<span style="color:#fa77ec">=></span> *We can override fixtures in multiple ways:*

	* <span style="color:#fa77ec">=></span> *We can override a fixture by creating a new fixture that holds the same name of the original one and pass the original fixture to the new one. or by using parametrization:*
	```
		@pytest.fixture
		def username():
			return 'username'

		*	@pytest.mark.parametrize('username', ['directly-overridden-username'])
			def test_username(username):
				assert username == 'directly-overridden-username'

		*	@pytest.mark.parametrize('username', ['directly-overridden-username-other'])
			def test_username_other(other_username):
				assert other_username == 'other-directly-overridden-username-other'
	```
