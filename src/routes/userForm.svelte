<script>
	import { Form, Field, Select, ErrorMessage } from 'svelte-forms-lib';
	import * as yup from 'yup';

	const formProps = {
		initialValues: { name: '', email: '' },
		validationSchema: yup.object().shape({
			title: yup.string().oneOf(['Mr.', 'Mrs.', 'Mx.']),
			name: yup.string().required(),
			email: yup.string().email().required()
		}),
		onSubmit: (values) => {
			alert(JSON.stringify(values));
		}
	};
</script>

<Form {...formProps}>
	<div class="user-form">
		<label>title</label>
		<Select name="title">
			<option />
			<option>Mr.</option>
			<option>Mrs.</option>
			<option>Mx.</option>
		</Select>
		<ErrorMessage name="title" />

		<label>name</label>
		<Field name="name" />
		<ErrorMessage name="name" />

		<label>email</label>
		<Field name="email" type="email" />
		<ErrorMessage name="email" />

		<button type="submit" class="btn-green">submit</button>
	</div>
</Form>
