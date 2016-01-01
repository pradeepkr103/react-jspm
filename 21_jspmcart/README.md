# Initial JSPM Example and Setup

## Quickstart

* Global
	npm install -g jspm
	This warns: Running jspm globally, it is advisable to locally install jspm via npm install jspm --save-dev.

*
	$ jspm init

* Get dependencies 
	$ npm install <something from anywhere> 

*
	W> load system.js & config.js in your index.HTML
	  - import your entry file and relax..

* Running 
	$ httpserver -p 1111   # somehow it must be run in same directory 


## Quick Ex1 - this directory

* In project dir # some need npm: or github: - but common packages registry simplifies
	$ jspm init
	$ jspm install jsx=npm:jspm-loader-jsx  #config.js would have jspm-loader-jsx named as jsx
	$ jspm install react react-dom
	$ jspm install css  # then can: import "./pathto/style.css!"


* index.html - has container for React DOM, and the system.js/config.js and top module app.jsx
	<div class="root"></div>

* app.jsx
	import React from 'react';
	import ReactDOM from 'react-dom';
	import CartItem from './cartItem.jsx!';

	const order = {
	    title: 'Fresh fruits package',
	    image: 'http://images.all-free-download.com/images/graphiclarge/citrus_fruit_184416.jpg',
	    initialQty: 3,
	    price: 8
	};

	ReactDOM.render(
	    <CartItem title={order.title} image={order.image} initialQty={order.initialQty} price={order.price}/>,
	    document.querySelector('.root')
	);


* cartItem.jsx
	import React from 'react';

	export default class CartItem extends React.Component {

	    static propTypes = {
	        title: React.PropTypes.string.isRequired,
	        price: React.PropTypes.number.isRequired,
	        initialQty: React.PropTypes.number
	    };

	    static defaultProps = {
	        title: 'Undefined Product',
	        price: 100,
	        initialQty: 0
	    };

	    state = {
	        qty: this.props.initialQty,
	        total: 0
	    };
	    
	    constructor(props) {
	        super(props);
	    }

	    componentWillMount() {
	        this.recalculateTotal();
	    }

	    increaseQty() {
	        this.setState({qty: this.state.qty + 1}, this.recalculateTotal);
	    }

	    decreaseQty() {
	        let newQty = this.state.qty > 0 ? this.state.qty - 1 : 0;
	        this.setState({qty: newQty}, this.recalculateTotal);
	    }

	    recalculateTotal() {
	        this.setState({total: this.state.qty * this.props.price});
	    }

	    render() {
	        return <article className="row large-4">
	            <figure className="text-center">
	                <p>
	                    <img src={this.props.image}/>
	                </p>
	                <figcaption>
	                    <h2>{this.props.title}</h2>
	                </figcaption>
	            </figure>
	            <p className="large-4 column"><strong>Quantity: {this.state.qty}</strong></p>

	            <p className="large-4 column">
	                <button onClick={this.increaseQty.bind(this)} className="button success">+</button>
	                <button onClick={this.decreaseQty.bind(this)} className="button alert">-</button>
	            </p>

	            <p className="large-4 column"><strong>Price per item:</strong> ${this.props.price}</p>

	            <h3 className="large-12 column text-center">
	                Total: ${this.state.total}
	            </h3>

	        </article>;
	    }
	}



* Run it ..
	$ http-server -p 7777
	W> http://localhost:7777/

* Production bundling
	$ jspm bundle-sfx app.jsx! app.js --skip-source-maps --minify

